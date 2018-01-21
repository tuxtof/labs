**************************************
Application Lifecycle Management (ALM)
**************************************

Overview
********

Application lifecycle management (ALM) is the supervision of a software application from its initial planning through
retirement. It also refers to how changes to an application are documented and tracked.

.. figure:: http://s3.nutanixworkshops.com/calm/alm/image1.png

Defining application lifecycle management (ALM) isn’t easy. Different people (and different vendors) take
quite different perspectives. Still, ALM is an important topic, and so understanding what it encompasses is
also important.

It’s common to equate ALM with the software development lifecycle (SDLC). Yet this simple approach is
too limiting; ALM is much more than just SDLC. In fact, an application’s lifecycle includes the entire time
during which an organization is spending money on this asset, from the initial idea to the end of the
application’s life. Application lifecycle management (ALM) is the supervision of a software application from its initial planning through retirement. It also refers to how changes to an application are documented and tracked.

.. figure:: http://s3.nutanixworkshops.com/calm/alm/image9.png

Application Portfolio Management
********************************

Application portfolio management (APM) is a framework for managing enterprise IT software applications and software-based services. APM provides managers with an inventory of the company's software applications and metrics to illustrate the business benefits of each application.

Application portfolio management is a critical part of IT governance. It enables organizations to inventory and manage all their applications in a highly transparent manner. 

Application Portfolio Management is an approach to managing IT software applications and software-based services across the enterprise. Mapping the universe of software applications against a common set of strategic drivers of investment allows you to make critical decisions about which applications to maintain, invest, retire or consolidate. Our approach to APM is centered on cost, performance and strategic alignment with business needs.

An APM system uses a scoring algorithm for generating reports about the value of each application and the health of the IT infrastructure as a whole. By gathering metrics like an application's age, how often it's used, the cost it takes to maintain it and its interrelationships with other applications, a manager can use more than just an educated guess to decide whether or not a particular application should be kept, updated, retired or replaced. 

Application Lifecycle Management
********************************

ALM is a very broad term that reflects a change in attitude towards software development that is also expressed in the term DevOps. DevOps blends the tasks performed by a company's application development and systems operations teams. In the past, a development team might work independently using a waterfall development model and hand off the completed software application to an operations team for deployment and maintenance. Today, it is more likely that developers will use an agile model and remain involved with the application after deployment, working with business owners and operations to make incremental changes as needed.  

ALM can be divided into three distinct areas: governance, development, and operations. The following figure 
illustrates this, showing each of these three aspects on its own horizontal line.

.. figure:: http://s3.nutanixworkshops.com/calm/alm/image10.png

Like a human life, an application’s lifecycle is demarcated by significant events. It begins with an idea:
"Why don’t we build something that does this?" Once the application is created, the next big event is
deployment, when the application goes into production. And finally, when it no longer has business value, the application reaches end of life and is removed from service.

1. **Governance:** which encompasses all of the decision making and project management for this application, extends over this entire time.
2. **Development:** the process of actually creating the application, happens first between idea and deployment. For most applications, the development process reappears again several more times in the application’s lifetime, both for upgrades and for wholly new versions.
3. **Operations:** the work required to run and manage the application, typically begins shortly before deployment, then runs continuously until the application is removed from service. Each of these three areas is important, and so each is worth examining in more detail.


Governance
==========

In ALM, the purpose of governance is to make sure the application always provides what the business
needs. The figure below gives a close-up view of ALM governance, providing a bit more detail on what it entails.

.. figure:: http://s3.nutanixworkshops.com/calm/alm/image11.png

The first step in ALM governance is business case development. As the above figure shows, this analysis happens
before the development process begins. Once the business case is approved, application development
starts, and governance is now implemented through project portfolio management. In some
organizations, this is simple: A project manager might be attached to the development team, or one of
the technical people on the team might take on this role. Other organizations use a more formal
approach, relying on a centralized project management office (PMO) to enforce established procedures.
Once the completed application is deployed, it becomes part of the organization’s portfolio of
applications. An application is an asset like any other, and so the organization needs an ongoing
understanding of its benefits and costs. Application portfolio management (APM) provides this, offering a
way to avoid duplicating functions across different applications. APM also provides governance for the
deployed application, addressing things such as when updates and larger revisions make business sense.
In fact, examining the APM section of the Governance line in more detail would show that it contains
business case development and project portfolio management for each of the revisions to the application
shown on the Development line.

Governance is the only thing that extends throughout the entire ALM time span. In many ways, it’s the
most important aspect of ALM. Get it wrong, and you won’t come close to maximizing the application’s
business value.

Development
===========

While equating ALM with the software development process isn’t accurate, development certainly is a
fundamental part of every custom application’s lifecycle. The following figure takes a closer look at this aspect of
ALM.

.. figure:: http://s3.nutanixworkshops.com/calm/alm/image12.png

Once the business case is approved, the software development lifecycle begins. If we expanded the SDLC
parts of the Development line shown in the figure, a modern process would probably show software
development as a series of iterations. Each iteration would contain some requirements definition, some
design, some development, and some testing. This iterative style of development isn’t always
appropriate—some projects are still better done using more traditional methods—but it’s becoming the
norm in many areas.

Once the SDLC process for version 1 of the application is complete, the application is deployed. For most
applications, however, deployment doesn’t mark the end of development. Instead, the application needs
periodic updates, as shown in the figure above, and perhaps one or more full SDLC efforts to create new
versions, as in this example. For some applications, the money spent on these updates and new versions
can exceed the cost of the original development by a significant amount.

Once again, notice the role of SDLC in the overall ALM process. As the figure above shows, this aspect is certainly
important, but it’s far from the whole story. Viewing ALM as synonymous with SDLC is just wrong—it
leads to a misunderstanding of what’s really required to be successful in this area.

Operations
==========

Every deployed application must be monitored and managed. The figure below shows some of the important parts
in this operations process.

.. figure:: http://s3.nutanixworkshops.com/calm/alm/image13.png

As with Governance, the Operations line is intimately connected to the Development line. For example,
planning for deployment likely begins shortly before the application is completed, and the act of
deployment itself is a fundamental part of operations. Once the application is deployed, it must be
monitored throughout its lifetime. Similarly, each update to the application must be deployed once it’s
completed, as the figure shows.

Application Release Management
******************************

Release management is a relatively new but rapidly growing discipline within software engineering. As software systems, software development processes, and resources become more distributed, they invariably become more specialized and complex. Furthermore, software products (especially web applications) are typically in an ongoing cycle of development, testing, and release, often running on evolving platforms with growing complexity. Such systems require dedicated resources to oversee the integration and flow of development, testing, deployment, and support.

In organizations that manage IT operations using the IT Service Management paradigm, specifically the ITIL framework, release management will be guided by ITIL concepts and principles. There are several formal ITIL Processes that are related to release management, primarily the Release and Deployment Management process, which "aims to plan, schedule and control the movement of releases to test and live environments.", and the Change Management process In ITIL organizations, releases tend to be less frequent than in an agile development environment. Release processes are managed by IT operations teams using IT Service Management ticketing systems, with less focus on automation of release processes.

**Build/Release**

A build is a software application which consists of a set of features and a few bug fixes and tested until it becomes stable. So basically it is a growing application in simple terms, the first build will have a few requirements and features in it. Lets say 10% of the software is developed. The next build will have bug fixes (ie. errors in first build is fixed) and also some new features are added. So lets say its now 20% of software is developed.

This process continues till 100%, ie. Until Build is stable.. Implies no bugs or very few bugs and all features have been developed. Which means it is a complete software, which is ready to use.  This final build is called a software application.

It is called a **Release**, when the client has agreed that they only need the basic features now in that software, because they cannot wait till all features are developed and the company developing the software can develop the next few features after the First Release (Software with basic features / Requirements of the client that has been satisfied)

**Continuous Deilvery**

Organizations that have adopted agile software development are seeing much higher quantities of releases[citation needed]. With the increasing popularity of agile development a new approach to software releases known as Continuous delivery is starting to influence how software transitions from development to a release. One goal of Continuous Delivery and DevOps is to release more reliable applications faster and more frequently. The movement of the application from a “build” through different environments to production as a “release” is part of the Continuous Delivery pipeline. Release managers are beginning to utilize tools such as application release automation and continuous integration tools to help advance the process of Continuous Delivery and incorporate a culture of DevOps by automating a task so that it can be done more quickly, reliably, and is repeatable. More software releases have led to increased reliance on release management and automation tools to execute these complex application release processes.

**Software QA**

QA tends to be focused on measuring and examining quality and improving the software through process improvements, thereby guiding the release to customers. Although testing activities usually do take place in this organization, the main focus of QA is on the processes and procedures of how software development activities take place.

QA is more focused on managing the product life cycle and verifying that the software meets the defined quality standards or customer agreements. QA is less about breaking the software and finding problems than about verifying that it is possible to make the software work under a given set of conditions.

**Software Test**

Testing, on the other hand, may keep an eye on the processes and often owns them, but is far more concerned with finding ways to break the software. Testers are to observe what the software does and to report on the level of quality as well as any serious issues they encounter.

Software Testing Processes:

- Unit testing: Performed to check the smallest units or modules of the product system at a time and is typically automated and repeated after each build. 

- Integration testing: to check whether two or more combined units/modules operate in a proper way.

- Functional testing to check the whole system behavior as per the defined requirements.

Testers must operate under the assumption that there are more bugs out there, and they have to find them. They operate in such a way that they expect to find problems, not just to verify that it is possible for everything to work fine. A good tester is one who is constantly thinking of things that have not been tried and is expected to exercise parts of the software that may be weak or that may not interact well. The whole point of this very critical look at software is to find bugs as fast as possible and get the right ones fixed. There will always be more bugs, but without knowing what they are, a conscious decision cannot be made regarding the software’s ability to meet the customer’s demands.

Test organizations can become flooded with bad software and drown in bugs if they are not going about their side of the process correctly. This will happen when a test organization becomes too reactive, only catching bugs instead of proactively preventing them. Individual testers may be required, above all else, to find more bugs. However, this focus on increasing the number of bugs instead of increasing the quality of the software can lead to the demise of many organizations.

A system that encourages subversion of the ultimate goal—making a software product that fulfills the customer’s needs—is not one to adopt. In a problematic system, testers are encouraged to find bugs after the bug has become part of the code base—when it is easy to quantify, and yet more expensive to correct than if it had been caught at an earlier stage. The problem with systems that reward individuals for meeting an intermediary goal is that people will achieve that goal instead of the ultimate goal.

The ultimate goal of any software development effort is to ship a high-quality product within a certain period of time and within a certain budget. Tasking individuals to find vast numbers of bugs may appear to be making progress towards the goal of making high-quality software, but that is not necessarily true. It is actually encouraging people to find problems in the software at a very late stage and to concentrate on finding symptoms instead of finding the core source of many symptoms.

Although many testers would never take advantage of a poorly constructed system, it still should not be set up in this way because it is not rewarding people for doing what management really wants. Failing to do that will eventually lead to an organization that has lost key members who saw past the reward system, leaving behind an organization that plays to management’s set of rewards.

Test organizations that are not effectively communicating with the rest of the software team (development and project managers) will not be aware of proposed changes and will not be able to step in early in the process to prevent problems, which allows a torrential flood of bugs to come back to testers late in the cycle and can end up costing the company time and money. Testing needs to evaluate processes as well as break the software.

Application Performance Management
**********************************

Application Performance Mamnagement, is largely an industry or vendor created term for anything that has to do with managing or monitoring the performance of your code, application dependencies, transaction times, and overall user experience.

.. figure:: http://s3.nutanixworkshops.com/calm/alm/image7.png

Since Application Performance Management is sort of a ubiquitous term for anything and everything performance related, some vendors use the term to mean totally different things, and can span several different types of vendor solutions.

- App Metrics based – Several tools use various server and app metrics and call it APM. At best they can tell you how many requests your app gets and potentially which URLs might be slow. Since they don’t do code level profiling, they can’t tell you why.

- Code level performance – Stackify Retrace, New Relic, AppDynamics, and Dynatrace are the typical type of APM products you think of, based on code profiling and transaction tracing.

- Network based – Extrahop uses the term APM in regards to their ability to measure application performance based on network traffic. There is a whole product category called NPM that focuses on this type of solutions.

Summary
*******

ALM is much more than just writing code. All three aspects—governance, development, and operations—
are important. Think about a project that gets the initial governance aspects wrong, for example, perhaps
by not understanding the business needs or failing to get the right stakeholders involved. No matter how
well the organization does development and operations, this project won’t provide much business value.
Similarly, a project that targets the right problems using a first-class development process might ignore
operational issues, such as providing enough resources to run the application reliably. Once again, the
business value this investment provides won’t be as large as it should be. Taking a broad view of ALM can
help organizations avoid problems like these.

Maximizing the value of the applications we create means doing all three aspects of ALM well. Achieving
this goal isn’t easy, especially when today’s ALM tools aren’t as well integrated as they could be. Yet
there’s no way around it: Taking a broad, holistic view of ALM is essential for improving this critical
business process.


.. |image0| image:: alm/media/image1.png
.. |image1| image:: alm/media/image3.png
.. |image2| image:: alm/media/image2.png
.. |image8| image:: alm/media/image8.png
.. |image7| image:: alm/media/image7.png






