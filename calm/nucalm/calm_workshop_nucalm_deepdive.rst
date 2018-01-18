********************
Calm Deep Dive
********************


NuCalm Entities in Aplos
************************

NuCalm uses a mix of Resource Intentful and Procedural APIs. This is due to the fact that all lifecycle operations on applications may not be intentful. 

**Note:** The challenges and use-cases of intetful API's are described in another document 

The following top level entities will exist in Aplos:

- /app_blueprints - Derives ResourceKind.
- /apps - Derives ResourceIntentful.
- /app_lca - Derives ResourceKind.
- /app_runlogs -  Derives ResourceKind.

**Introducing “Locks”**

The IntentSpec entity will require additional attributes:

- lock (is a flag - none-unlock or 1-lock)
- locked_by (will be component name locking the entity)
- locked_timestamp_secs
- ttl (time after which Intent Gateway should take action on the spec).

These attributes can be sent in the opaque_data field of the intent_spec by the caller using the create_or_update_intent_spec method. Any component that wishes to perform a transaction (nested or otherwise that requires a lock) on any entity kind will be able to take a lock on the entity in the IntentSpec. 

The Intent Gateway will respond to any PUT or POST requests on the entity with an appropriate response stating that “Entity is Locked” (423 Locked (WebDAV; RFC 4918) The resource that is being accessed is locked). The component post completion of the transaction should update the final spec and unlock the said entity in the IntentSpec by clearing all the fields. Any GET requests will be handled by Aplos/Aplos Engine with the current spec and status accordingly.

The component may not lock all the nested entities when the transaction is initiated. For e.g. the app spec when created in the intent_spec entity kind will be locked. The nested entity the VM will be locked as part of the create_action of the VM. The nested entities too shall have to be locked. Once the application create is successful the application entity and associated entities will be unlocked together along with the final spec.  Batch APIs can be used to complete the unlocking operation.

In case of failure, the Component shall update the IntentSpec with the spec of the Entity as it exists at point of failure. **Entity**,  **Spec**, and **Status** in such cases may or may not match. User will trigger the necessary actions to ensure that the Application finally results in a steady state.

.. figure:: http://s3.nutanixworkshops.com/calm/nucalm/image11.png

NuCalm Components
*****************
- API Service - Styx
- NuCalm Engine - Jove, Hercules & Iris
- Execution Engine - Epsilon

Engine Design
*************
Engine is comprised of 3 component: A Manager - **Jove**, set workers - **Hercules**, and a callback listener - **Iris**. There can be one manager and multiple workers per node. Across nodes, only one manager is active (or a leader) at a given point in time. All worker process in a PCVM connect to the local manager when they are brought up. The associated manager will maintain the “connected worker pool”. The styx/http endpoint will contact the elected manager for any run action processing. A request to engine can be of two types viz., run an action, abort an already running action. The engine will convert the action using contextual information available from application to workflows processable by orchestration engine and starts the run of corresponding workflows.

.. figure:: http://s3.nutanixworkshops.com/calm/nucalm/image15.png

Here are the processing steps of request:

1. Styx finds out the current Jove leader.
2. Request sent by Styx to leader Jove on a pre-defined port. 
3. The received request will be processed by the HTTP endpoint of Jove. The manager will enqueue a packet of running an action with relevant data(as workstate proto) in ergon.
4. The scheduler in manager will create a task in Ergon and sends the taskid to one of the connected Hercules(es). Hercules will mark the task as completed after it has dispatched work to the Epsilon (orchestration engine) with configured callbacks to Iris. Epsilon will send requests to Iris with current run information.
5. Communication between Jove and Hercules will be via a bidirectional stream. The connection is initiated by Hercules, served by Jove. The message to process next item to worker is pushed by Jove & Hercules sends back ACKs.  If Hercules chooses to send a request to do something more, it has to send through the HTTP endpoint exposed out. The protocol of communication initially begins as HTTP and is upgraded to WebSockets. The binary messages sent over WebSockets are serialised protobuf messages.
6. As Epsilon Engine picks up the workflow run request & processes it, it sends notifications to Iris & Iris in turn updates/creates the relevant entities in IDF.

App Call Flows
**************

.. figure:: http://s3.nutanixworkshops.com/calm/nucalm/image13.png



app_blueprints
**************

AppBlueprint will be derived from Resource class and all API calls will be procedural in nature.
Users will be able to create, edit and delete app_blueprints. The blueprints can be in various states while it is being created/edited. Its various states can be Draft,  Compiled,  Active, InActive,  Deleted,  Error. Once the blueprint is successfully compiled and verified it can be launched to create an App.

**app_blueprints → app.spec Generation**

When the blueprint is launched using POST /app_blueprints/{uuid}/launch, following things happen:

1. Styx will receive the blueprint (Say BlueprintA) from which the application (say AppB) is to be instantiated.

2. Blueprint-A will be cloned with a new name and uuid - Blueprint-A_for_AppB

3. Styx will then generate the app spec from BlueprintA_for_AppB

4. Styx will post locking Intentful call to Aplos POST /apps

5. Aplos will forward the call to Aplos Engine where the app plugin will in turn forward it to Styx

6. Styx will make an create action call to Jove

**app.spec parsing and runbook generation**

Jove will generate the app spec and runbooks for its entities. It does so by carrying out the following:

1. Jove populates the user inputs within BlueprintA_for_AppB

2. Generate live entities within IDF

3. Parse the spec for resolving macros

4. Generates dependencies, implicit and explicit

5. Generates runbooks for all system actions or invokes the create runbook already existing (This create runbook is generated in case the blueprint is compiled) 

**Runbook → Epsilon workflow generation**

The following steps be carried out by the workflow generator:

1. It would be to create entities with Epsilon for App, Deployments, Substrates, Packages and Actions

2. Generates the workflows from the actions for Epsilon to execute

3. Creates a run log entry into IDF for execution of the action

4. Initiate execution of the workflow for the corresponding action in Epsilon. 

**Note:** The assumption here is that epsilon in PCVM would be able to reach the VMs in all deployments. This needs more discussion with the infra team.

**LCA Execution** 

LCA Execution involves Epsilon and Iris: 

1. Epsilon will begin execution of the workflow. 

2. Epsilon will regularly send updates back to Iris (Runlog listener) at various stages of workflow execution. Especially when attributes or properties are needed to be set for Calm entities.

3. Iris in turn will update the run logs to indicate the progress of the action.

4. Iris will update the attributes and state of the app and its associated entities in IDF.

5. Once Iris receives a notification that workflow is complete it will update the spec for the entity in IntentSpec and unlock the app and its entities.

6. Epsilon has to store task output on a shared filesystem provided by the platform. The filenames would have to be generated and stored using UUIDs. Later, once the platform has ElasticStack, epsilon would push the outputs there.
 

Macros
******

**Syntax**

For attribute/property access of other objects:
                @@{<Object-Name>.<variable/attribute name>}@@

Ex: @@{my_vm.vm_ip}@@. Fetch “vm_ip” property from “my_vm” object which may either be any one of the supported object types.

Supported Objects

- Application
- Deployment
- Service
- Package
- Substrate

Property access across applications is not supported for now. 
                 
**E.g.:** @@{Deployment.vm_ip}@@

Fetch “vm_ip” from the deployment on which the current script is being executed without having to know the name of the deployment directly.

Built-in macros
***************

Similar to the previous implementation, prefix can be changed.

+----------------------------------------------+--------------------------------------------------------+
| @@{calm_array_index}@@ .                     | Index of the object within the array                   |
+----------------------------------------------+--------------------------------------------------------+
| @@{calm_blueprint_name}@@                    | Name of the blueprint that the app was created from    |
+----------------------------------------------+--------------------------------------------------------+
| @@{calm_blueprint_uuid}@@                    | UUID of the blueprint that the app was created from    |
+----------------------------------------------+--------------------------------------------------------+
| @@{calm_application_name}@@                  | Name of the application                                |
+----------------------------------------------+--------------------------------------------------------+
| @@{calm_application_uuid}@@                  | UUID of the application                                |
+----------------------------------------------+--------------------------------------------------------+
| @@{calm_project_name}@@                      | Name of the project that the application belongs to    |
+----------------------------------------------+--------------------------------------------------------+
| @@{calm_is_within("time1", "time2")}@@       | Return '1' if the current time is within the supplied  |
|                                              | time1 and time2 range                                  |
+----------------------------------------------+--------------------------------------------------------+


Implementation
**************

A translation layer in calm will expand these macros to either their actual current value or to epsilon macros to fetch their values at run time.

The decision of whether to evaluate at run time or before running will be made based on dependencies, task outargs and consolidated runbook outargs (in case of call runbook tasks).

For example:

- There are 2 services S1 and S2.
- A script running on S1 requires ‘vm_ip’ property of S2.
- The ‘vm_ip’ property of S2 is task outarg for some tasks (in this case, vm actions).
- When a runbook is built, we will build a map of object name to outargs of tasks in the current runbook.
- If ‘vm_ip’ is not present in any task/runbook outarg on S2 as part of the current runbook, then the macro is evaluated before the run, else it translates to a property access macro i.e. @@{AZ_LIST(Machine(“S2”)).get(Property(“vm_ip”))}@@

**How it works?**

Pass the script to a Lexer which will have two states:

*INITIAL*

The only token here will be ‘@@{’ which will switch the lexer to ACTIVE state, the rest of the buffer is all ignored.

*ACTIVE*

Continue to grab everything from the buffer until ‘}@@’ which will switch the lexer back to INITIAL state.

The part of the buffer that we grabbed in ACTIVE state can be evaluated directly without any special grammar for now, later this can be changed when we have the need for more complex stuff.



Calm to Epsilon object mapping
******************************

Every calm object maps to a corresponding object in Epsilon.

**Epsilon entities**

- The Calm Application itself 
- Each Deployment (Group) 
- Each Substrate (Group) 
- Each Package (Group) 
- Each Service (Group) 

**Epsilon machines**

- One machine shared between Deployment Element, Substrate Element, Package Elements & Service Elements i.e. one machine per replica.

**Properties on Epsilon objects**

Entity properties in epsilon are a consolidated list of values for each property on individual elements. Since the machine is shared between multiple elements, namespacing is used to maintain uniqueness of property names. 

Namespace for a Calm object is: ‘<Calm_Object_Type>_<Calm_Object_Name>_’, for a deployment d1, properties on the epsilon machine would look like this: ‘Deployment_d1_<prop_name>’.

No namespacing is done for Entity properties, but the namespace is used to filter props belonging to that Group entity when consolidating props from machines.

**Epsilon lib changes for partial updates (PATCH support)**

Earlier, machines and entity apis only supported a full update. This meant that if two epsilon workflows running in parallel tried to update the same machine, at the same time, the last update would overwrite the previous one.

This behaviour has been changed for property updates (Entity props and machine props) by allowing partial updates. These changes are present in epsilon lib, zaffi, and pyepsilon.

**Example**

.. codeblock:: python

  Task 1
  z = get_handle()
  props = [
      {
          ‘name’: ‘a’,
          ‘value’: ‘1’
      }
  ]
  m = z.Machine(uuid=’xyz’)
  m.update_props(props)
  
  Task 2
  z = get_handle()
  props = [
      {
          ‘name’: ‘b’,
          ‘value’: ‘2’
      }
  ]
  m = z.Machine(uuid=’xyz’)
  m.update_props(props)
  
  
Now if Task 1 and Task 2 were to execute parallely, the resulting props on the machine would be:

.. code-block:: json

 [{‘name’: ‘a’, ‘value’: ‘1’}, {‘name’: ‘b’, ‘value’: ‘2’}]
 
The update_props api and Namespacing make it possible to have a shared Epsilon machine/entity between multiple Calm objects.

**Macro Translation**

To take care of property inheritance, namespacing, and dependencies we need to translate all macros to Epsilon readable macros. 

**How and where it happens**

When building the workflow for any action (action.build_wf()), macro translation involves the following:

- Building macro_context for the current application and the current action
- Macro parsing on a per task basis which happens during task serialization
- AST generation and evaluation

**Macro context**

The Macro context contains the relationship between calm objects. This is how it looks:

Application
     ↓
Substrate(s)
     ↓
Deployment(s)
     ↓
Package(s)
     ↓
Services(s)
     ↓


The reason Substrates sit above deployments is because all objects inherit properties of the substrate.

**Parser**

The parser has 2 modes - the first is a lexer switch to consume everything in the buffer till a trigger (‘@@{’) is encountered and the second is a parser that will parse everything from ‘@@{‘ upto ‘}@@’ to generate our ast. Everything other than properties, built-in macros and dependencies is ignored, Epsilon’s macro parser will take care of that.

**Translation of properties**

Epsilon does not understand Calm’s object model and property inheritance. Therefore, for a Task running on a certain service, if you need to access a certain property coming from some other task that ran on the substrate, for example, say @@{ip_address}@@, it needs to be translated to @@{Substrate_<substrate_name>_ip_address}@@ for epsilon to correctly expand the macro the the relevant one coming from the substrate.

**Translation of dependencies**

Despite entities inheriting properties, a dependency from one Calm object to another doesn’t generate any parent-child relationship on the corresponding epsilon entities, which is why macros need to be translated to something epsilon can evaluate, i.e. AZ_LIST(Entity(uuid=<uuid>).get(Property(“<prop_name>”))).

Dependencies
************
Dependencies in nuCalm only hold for system actions. They can be expressed in three ways:

**Dependency Types**

- Inherent dependencies:

   These are inherent to the model and no specification is necessary.
   For eg. Substrate has to be created before packages and services. Services have to be stopped before package    is uninstalled etc. In terms of dependencies it translates into services depend on their packages which both    depend on the underlying substrate. They are inherent to the system and used by system actions.

- Explicit dependencies:

   - They are expressed by depends_on list in the config section of different calm entities.
   - Used only by system actions.


   For eg. Theoretically **depends_on** list of **S2** can be as follows **[S1, SUB10, DEP4]** where **S1** and **S2** are part of same deployment, **SUB10** is a part of another deployment and **DEP4** is yet another deployment. But in usage the **depends_on** list on a service will have only other services because it is the logical unit we want to expose.
   
   In the application context **S2** can be created/started only after service **S1**, substrate **SUB10** and **DEP4** are created/started and should be deleted/stopped after the other three entities are deleted/stopped. 

   In terms of system actions on application it means that create **RB** of **S1**, **SUB10** and **DEP4** will be run before the create **RB** of **S2**. A dependency edge from **S2** to **S1** translates into an orchestration edge from create/start CallRunbookTask of **S1** to CallRunbookTask of **S2**.

   When a system action is run in the context of a deployment, only the entities in the depends_on list which are a part of this deployment are used.

   When an action is run in the context of the service **S2** alone, these dependencies don’t hold. We will not enforce any of the dependencies.

- Implicit dependencies calculated by the usage of macros in tasks:

   These dependencies are created within the context of a system action. Tasks can use variables and certain attributes from other entities. Mere usage of a variable in a task does not translate to a dependency. When a macro is used in a task and another task in the action sets the same, it translates in a dependency and an orchestration edge (the reverse of the dependency edge) in an action. A getter and setter on a variable have to be a part of the action.

   These dependencies are also possible only in system actions. The dependency from a task to another when they are not in the same callrunbooktask, translates into an orchestration edge of the callrunbook tasks they are a part of. Edges between tasks across calmrunbooktasks are not possible in system actions. 

   So when a dependency is created between tasks **T1** (getter) and **T2** (setter) which are are part of same callrunbooktask, the orchestration edge is created between **T1** and **T2** *(T2 → T1)*

   When a dependency is created between two tasks **T1** and **T2** which are part of different callrunbooktasks **CT1** and **CT2**, we have to traverse up the chain till we get the first callrunbooktask and create an orchestration edge **CT2 → CT1**

   The implicit dependencies between callrunbooktasks of a create action will be promoted to the depends_on list so these can be used by other system actions. 

**Dependencies as presented to the user (status sections)**

Dependencies in system actions are present to the user as a dependency list. This list is used to show dependency edges in the UI.  This list has no use for generating the orchestration edges. 

dependency_list:
    - getter_resource_kind: <enum>		
    - getter_resource_name: <string>
    - setter_resource_attr: <enum>	
    - setter_resource_kind: <enum>
    - setter_resource_name: <string>
    - action_resource_kind: <string>
    - action_resource_name: <string>
    - action_name: <string>
    
When actions are generated, the use of setter and getter tasks translates into orchestration edges between the parent callrunbooktasks and the dependency_list is generated for use in the topology view. 

**Dependencies and System generated Actions**

- Actions will have an additional field which indicates whether it is system generated or user edited.

- System actions will all be generated together at the compile step and will be marked as system generated and presented in the status section.

- If action is edited by the user and it presents itself in the spec section.
   - we evaluate it for cycles and put it in the db and mark it as user edited.
   - We generate all the actions that are impacted by change and mark them also as user edited.
      - When create-action Is edited all other action edges are rebuilt based on this one and marked as user edited.
      - When an action x for an entity is edited all the action x for other entities are rebuilt and marked as user edited.
   - At this point, the action edges can be inconsistent with the other settings like dependency.
- Every time a spec change which can upset the edges happens, either depends_on list changes or script changes (more generally dependency list changes) we warn the user that the action edges created earlier will be thrown away and we will rebuild the edges and mark the actions as system generated again. At this point the user can choose to not proceed and withdraw the spec changes.
- But if the action spec changes are such that they don’t upset the dependency list,  (variable changes or direct edge changes) we accept the change and put the action in the db and keep the flag as user edited.
- A system action cannot be deleted. 

We generate actions from different spec params which can change independently and the generated action itself can be edited independently. So we have to impose restrictions with this flexibility. Change in action edges directly replaces action to user edited. Changes in dependency list rebuilds the edges and marks the action as system generated. 

To summarize, if the dependency list changes after user has edited any of the generated actions, user-edited actions should be thrown away and system should rebuild all actions.

**Dependencies and User Actions**

No orchestration or dependency edges will be created based on any type of dependency - inherent, explicit or implicit.

In a user action, the use of setter and getter task do not translate to orchestration edges between those tasks. The user will have to explicitly draw an edge. 

Handling Secrets
****************

**API Format**

Generic Secret String

Any “<secret string>” type in spec should follow the below structure:

.. code-block:: json

  {
      "<secret_key_name>": {		# e.g. - password in credentials, secret_access_key in accounts
          "attrs": {
              "is_secret_modified": "<boolean>",
              "secret_reference": {	# secret reference section to handle secrets exposed as top-level entities
                  "uuid": "<uuid>",
              },
          "value" : "string", 		# used only for POST/PUT. During PUT, modified bit in attrs should be set to      True. A GET call does not fill in the value and resets the modified bit to False.
      },
   }
   
Later, if /secrets (or equivalent) are exposed as a top-level entity in Aplos, **uuid** in secret_reference can be used to update the secrets independently.

**Secret Variable**

Secret variables are supported in nuCalm. Any secret variable should follow the below structure:

.. code-block:: json

  {
      "uuid": "<uuid>",
      "description": "string",
      "label": "string",
      "name": "string",
      "type": "secret",
      "attrs": {
          "is_secret_modified": "<boolean>",
          "secret_reference": {
              "uuid": "<uuid>",
          },
      },
      "val_kind": "string",
      "var_type": "local",
      "value": "",
  }

The attrs field in variables would be used to store secret related attributes. All secret variables should be available as macros like any other variables.

**Credential**

A credential is used in nuCalm to store sensitive information like passwords/keys. These details are used connect to a user/third party system. From the AppSpec doc, the way to define a credential is given as follows:

.. code-block:: json

  name: <string>
  uuid: <uuid>		       
  type: <enum> 		# passwd, key
  username:
  secret: <secret string>

where secret follows the generic secret type mentioned above. So, an expanded view would look like:

.. code-block:: json

  name: <string>
  uuid: <uuid>		       
  type: <enum> 		# passwd, key
  username:
  secret:
      value: <string>
      attrs:
          is_secret_modified: <boolean>
          secret_reference:
              uuid: <uuid>
    
username and secret fields in a credential would be available as macros. A user should be able to use @@{<credential_name>.username}@@ and @@{<credential_name>.secret}@@. 

**Storing Secrets**

All secrets should ideally be stored using a vault like service. As such a service does not exist yet, as a starting point all secrets should be encrypted (AES) and stored in a seperate collection in the db. Some points to be considered while storing secrets:

- Encryption should fully comply to the standards defined by security team.
- All objects using secrets should only store its uuid for reference.
- Secrets should never be returned as part of any API calls.
- Secrets should never be returned as part of export.
- Any scripts using macros which are secrets should never store the raw script file. It should mask all secrets before storing.

Deployment Details
*******************

**Packaging Mechanism - RPM Packages**

The following rpm packages would be built as part of nucalm continuos integration process:

1. nucalm-engine
2. epsilon

These packages would be stored in a private yum repository.

**Delivery Mechanism - Docker Container Images**

Docker images would be built using DockerFiles which would pull the respective rpm packages built in the previous step. These images would derive from a base container image provided by nutanix infrastructure. These images would be pushed to a private docker registry for consumption.

Dependencies

Services required by nucalm:

- IDF
- Uhura
- Zookeeper
- Ergon
- Epsilon
- Aplos
- ElasticStask

Services dependent on nucalm:

- Aplos

**nucalm-engine** and **epsilon** would register with service discovery when they are run. Similarly, the dependent services would be discovered using platform’s service discovery mechanism (The assumption now is that it would use zookeeper).

.. |image0| image:: nucalm/media/image11.png
.. |image1| image:: nucalm/media/image15.png
.. |image2| image:: nucalm/media/image13.png
