# Group project

## Table of contents
- [Web Application](#web-application)
  * [Introduction](#introduction)
  * [Front-end](#front-end)
  * [Back-end](#back-end)
- [Agile method](#agile-method)
  * [What is Agile?](#what-is-agile)
  * [What Agile method did we use?](#what-agile-method-did-we-use)
    + [What could we have done to improve our workflow?](#what-could-we-have-done-to-improve-our-workflow)
      - [Definition of Done](#definition-of-done)
      - [Pair programming](#pair-programming)
  * [What do I think of Agile?](#what-do-i-think-of-agile)
- [Cultural differences and ethics](#cultural-differences-and-ethics)
  * [Ethics](#ethics)
    + [What is ethics in software engineering?](#what-is-ethics-in-software-engineering)
    + [Why is ethics important in software engineering?](#why-is-ethics-important-in-software-engineering)
    + [What do you have to do as a software engineer to address ethical aspects in your work?](#what-do-you-have-to-do-as-a-software-engineer-to-address-ethical-aspects-in-your-work)
      - [Which ethical guideline do I think is most important?](#which-ethical-guideline-do-i-think-is-most-important)
    + [How do you know that your ethical considerations match with those of other software engineers?](#how-do-you-know-that-your-ethical-considerations-match-with-those-of-other-software-engineers)
    + [Which ethical aspects play a role in your project?](#which-ethical-aspects-play-a-role-in-your-project)
    + [Do you foresee ethical conflicts caused by your software? What kind of?](#do-you-foresee-ethical-conflicts-caused-by-your-software-what-kind-of)
    + [Can you do something to avoid or minimize these conflicts?](#can-you-do-something-to-avoid-or-minimize-these-conflicts)
  * [Culture](#culture)
    + [What is culture?](#what-is-culture)
    + [Which are well-known dimensions on cultural differences?](#which-are-well-known-dimensions-on-cultural-differences)
    + [Can you give examples for cultural differences that you have experienced in your study or life? How do you explain these differences?](#can-you-give-examples-for-cultural-differences-that-you-have-experienced-in-your-study-or-life-how-do-you-explain-these-differences)
      - [Mannerism](#mannerism)
      - [Food](#food)
      - [Religion](#religion)
    + [What is your culture?](#what-is-your-culture)
- [Requirements and design](#requirements-and-design)
  * [Requirements](#requirements)
  * [Design](#design)
- [Business process](#business-process)
  * [What is a business process?](#what-is-a-business-process)
  * [Analysis](#analysis)
  * [How could this be used in other projects?](#-how-could-this-be-used-in-other-projects)
- [Professional](#professional)
  * [Communication & feedback](#communication--feedback)
  * [Source control & code quality](#source-control--code-quality)
- [Sources](#sources)

## Web Application
The goal of this learning outcome is to design and build a full-stack web application using a JavaScript framework in the front-end and an Object Oriented framework (.NET or Java) in the back-end. This application should use a distributed software architecture, Object Related Mapping and relevant communication protocols.

### Introduction
Our group project has been provided by ISAAC. Spread across the entire office of ISAAC are small temperature sensors that periodically send out temperature and humidity readings. At the time of writing this, these readings are only used for sending out push notifications to the office manager if the temperature becomes too high. However, ISAAC would like to do more with these sensors. As such, the company has asked us to create an application that can create heatmaps based on these temperature readings of a given period.

On top of this, the company has small interactive screens positioned next to the entrance of all meeting rooms. Currently, these display only a static number, but should be populated with the most recent temperature reading of that room.

### Front-end
Our project contains two front-ends, each serving a different purpose and running on a different system. The first front-end makes use of a JavaScript framework, namely Vue, and has the express purpose of showing the most recent temperature reading of the sensor closest to it. This front-end runs as a part of the interface of the appointment screens located near the entrance of all meeting rooms.

The second front-end does not make use of a JavaScript framework, instead using plain HTML. This front-end uses Axios to communicate with the back-end application and receive all temperature readings from a specific date. Highcharts is then used to generate and visualize a heatmap based on these temperature readings. A slider can be used to ajust the specific point in time for which to show a heatmap, and a share button can be used to share this record with others.
![Front-end](https://i.imgur.com/EdN8VXt.png)

### Back-end
The back-end project consists of a single microservice that is directly accessed by the front-end. We decided to use the .NET Core framework for our back-end project, and is written in C# because of it. The microservice consists of the following three layers:

- The API layer, responsible for listening for, decyphering and responding to incoming requests.
- The service layer, containing all microservice-specific logic.
- The repository layer, responsible for making database calls using the ORM.

In order to allow individual layers to freely be replaced/changed, abstraction has been implemented using the factory method. Database calls are made using Entity Framework as the ORM, but is interchangeable because of the implementation of the repository pattern.

## Agile method
The goal of this learning outcome is to get an understanding of what Agile is, how it works and work with it in a project.

### What is Agile?
[Agile](https://www.agilealliance.org/agile101/) is a method of project management and software development. It aims to improve the workflow of a project by working in small, incremental periods of time where communication and evaluation of the collaboration, progress, issues and (unexpected) developments play a vital role. In doing so, it hopes to achieve an environment where all parties involved can quickly adjust to new situations.

The agile method of working has been implemented in software engineering through the use of various Agile frameworks. Commonly used frameworks include:
- **Scrum**: A beginner-friendly Agile framework where a team works in small iterations with much communication between the team members and with the stakeholder. 
- **Kanban**: A framework with freedom at it's core. While the framework does include a planning board, the team is free to build on top of this as they see fit.
- **Extreme Programming**: A programmer-oriented framework similar to Scrum. It implements a number of values and techniques that helps improve the workflow of software developers and improve the quality of the resulting code.
- **Feature-Driven Development**: An Agile method that revolves around analyzing, designing, implementing and testing the features of a program.

The goal of these frameworks and working methods is to facilitate the people involved rather than the end result. It allows development teams to approach a project from various angles and disregard traditional roles and hiarchies in favour of involving those with the right skill set. While each framework provides a base, common practices can be used to further build upon this base. Examples of common practices include **pair-programming**, **test-driven development**, **stand-ups/downs** and **sprints**.

### What Agile method did we use?
From the very start of the project, we decided to adopt an Agile method of working. Ultimately, we chose to use the SCRUM framework, because we judged it to be best suited for our software development project and easiest framework to get accustomed to.

At the start of each sprint, we would start planning ahead on what we would like to achieve in the next sprint. We would do this by writing new tasks on the sprint planning board for that sprint, prioritizing them and assign members to the ones that would be picked up immediately.

![Sprint planning board](https://i.imgur.com/IKzCzsV.png)

Every day we worked on the group project, we would have a stand-up, in which we all discussed what everyone would be doing that day, the expected outcome at the end of the day and what we would need to get there. At the end of each day, we would also have a daily stand-down, where we discussed the progress we made, as well as possible roadblocks we encountered and update the sprint planning accordingly. We would then take this into account during the following day’s stand-up.

![Task list](https://i.imgur.com/IrfNU3H.png)

At the end of each sprint, we would have a sprint review with the stakeholder in which we would present our progress and receive feedback. After this, we would have a retrospective in which we filtered through all feedback, decide what we would do with it and update our planning accordingly. We would also use this moment to discuss our teamwork, what went well and what we could do differently during the next sprint.

#### What could we have done to improve our workflow?

##### Definition of Done
While we did work using the Agile method during this project, there are various areas in which we could have improved or added upon. One such area would be creating our definition of done. While we did have a list of requirements for each task to be implemented before it could be considered complete, we did not have a global definition that applied to all tasks. This in turn created lackluster quality control, with few to none tests being written.

##### Pair programming
One thing I feel like we could have benefitted from if we had implemented it in our development process is pair programming. During our project, there were various occurrences of two people working on the same (or a closely related) task. With pair programming, both members could have given live feedback to one another while in turn learning about the other's coding style and the structure of the code they wrote. This in turn would have prevented a situation in our project where one of the two members working on the back-end project lost track of the code's functionality during the later stages of the project. 

### What do I think of Agile?
As someone who has used the waterfall method in past educational projects, the modularity and ease at which Agile can be applied to a software development makes for a very pleasant change of phase. The lack of most roles introduces a sense of equality which is unrivaled, and allows for team members to be selected based on their skills instead, which I am a very big fan of. On top of this, the continuous communication between team members does not only improve the workflow (by knowing everyone's progress, roadblocks and uncertainties), but in my experience also helps tremendously with building up connections with the team members.

While we certainly did not fully adhere to the Agile principles during our project (the lack of a concrete Definition of Done for the majority of the project for example), this did not impact my opinion of it in a negative way. I very much liked working using SCRUM as a base, and, when the time presents itself, would like to use it again in a different environment while attempting to rectify the mistakes made during this project.

## Cultural differences and ethics
The goal of this learning outcome is to get an understanding of ethics and cultural differences, as well as their position and value in a software engineering environment.

### Ethics

#### What is ethics in software engineering?
Ethics, as described by Oxford Languages, is a set of moral principles that govern the behaviour of a person or the conduction of an activity. Ethics are used to measure the righteousness of an action, acting as criteria for judging whether something is right or not. In the scope of software engineering, ethics dictate the values a software engineer should adhere to in a professional environment. This includes ethics related to cultural differences, software, source code and communication.

#### Why is ethics important in software engineering? 
Ethics in software engineering can be viewed from multiple perspectives. One of these perspectives is that of the production environment. Another one would be that of the product (and it’s users).

The ethical values adhered to during the production of a product define the workability and comfortability of the environment. A lack of proper ethical values or a lack of enforcing them will eventually create a working environment where discrimination, bullying, favouritism among other things are allowed. For this reason, it is important for proper, unanimously agreed upon ethical values to be present.

The ethical values the product adheres to at it’s core define how an application could impact it’s users. An application built with strong ethical values at it’s core should result in a product that will affect users in a positive manner, whereas one that lacks this core may result in the opposite reaction. For this reason, it is important to consider how users could be negatively impacted by something, and what can be done to minimize (or negate) this reaction.

#### What do you have to do as a software engineer to address ethical aspects in your work?
Within the software engineering space, a number of widely used ethical guidelines can be found on the internet. One such guideline is the [ACM Software Engineering Code of Ethics](https://ethics.acm.org/code-of-ethics/software-engineering-code/), which states that software engineers should strife to analyze, design, develop, test and maintain software in a truthful, upfront and professional manner for the better of public health, safety and welfare of humanity, according to the following eight principles: 

```
1. PUBLIC – Software engineers shall act consistently with the public interest.
2. CLIENT AND EMPLOYER – Software engineers shall act in a manner that is in the best interests of their client and employer consistent with the public interest.
3. PRODUCT – Software engineers shall ensure that their products and related modifications meet the highest professional standards possible.
4. JUDGMENT – Software engineers shall maintain integrity and independence in their professional judgment.
5. MANAGEMENT – Software engineering managers and leaders shall subscribe to and promote an ethical approach to the management of software development and      maintenance.
6. PROFESSION – Software engineers shall advance the integrity and reputation of the profession consistent with the public interest.
7. COLLEAGUES – Software engineers shall be fair to and supportive of their colleagues.
8. SELF – Software engineers shall participate in lifelong learning regarding the practice of their profession and shall promote an ethical approach to the practice of the profession.
```
##### Which ethical guideline do I think is most important?
While all eight of these principles are equally important, I personally think principle #3 is the most important of all of them. The product is that which will be used by the end user, be that an internal company employee or anyone on the internet. The ideas and values which were used to create the product, as well as the degree of professionalism and care put into the process could impact the user experience immensely. Depending on the type of product, if the end product is lacking in this regard, it could potentially have catastrophic effects on the end-user.

#### How do you know that your ethical considerations match with those of other software engineers? 
In order to verify all software engineers’ ethical values match up, it is important for all parties involved to converse about the topic. Allowing everyone to share their ethical values and boundaries with one another makes it possible to create a working environment in which all parties involved can work comfortably.

#### Which ethical aspects play a role in your project?
One of the ethical aspects in our group project that we faced was the licensing of third party libraries. As our project lives in a gray area between commercial and non-profit (the organization is a business, while the application is strictly for internal use without any direct link to generating profit), we contemplated whether purchasing a license for these libraries would be necessary.

#### Do you foresee ethical conflicts caused by your software? What kind of?  
I personally do not expect any ethical conflicts in our project, as our application does not accepts little user input and no user generated data is stored in the database of the back-end application. The back-end application will also not accept any (purposefully) received incorrect/malformed information.

#### Can you do something to avoid or minimize these conflicts? 
Communicating regularly with one another is key in preventing (or solving) conflicts in a professional setting. Expressing one’s concerns and problems spreads awareness to all parties involved and allows them to seek out a solution together. It also minimizes the amount of misunderstandings that could potentially happen.

### Culture

#### What is culture?
Culture, as defined by Oxfort Languages, is the collective ideas, customs and social behaviour of a particular group of people or country. This includes topics such as food, mannerism, clothing, (social) values and religion, among other things.

#### Which are well-known dimensions on cultural differences? 
A variety of well-known lists of cultural dimensions exists. One such list is the [Trompenaar Model of Cultural Dimensions](https://expertprogrammanagement.com/2017/10/trompenaars-cultural-dimensions/). This model consists of seven dimensions which can be used to differentiate cultures by preference. Following is a list of these dimensions:

```
1. Universalism vs particularism - Defines whether rules or relationships matter more.
2. Individualism vs communitarianism - Defines whether those of a certain culture prefer working alone or in teams.
3. Specific vs diffuse - Defines how those of a certain culture differentiate between their personal and professional lifes.
4. Neutral vs affective - Defines how (or if) those of a certain culture show our emotions.
5. Achievement vs ascription - Defines whether status is achieved or given.
6. Sequential time vs synchronous time - Defines whether those of a certain culture perform one task at a time or multiple tasks alongside one another.
7. Internal direction vs. external direction - Defines whether or not people of a certain culture take control of the environment they live in, or if they let the environment control them.
```

#### Can you give examples for cultural differences that you have experienced in your study or life? How do you explain these differences?  
While I have no notable experiences with different cultures, there are various differences between the Dutch culture and those of other countries.

##### Mannerism
One aspect that varies widely are how people act, and what they find appropriate in different cultures. Instead of shaking hands, countries such as Japan prefer to greet others using a bow instead.

##### Food
Food is another aspect that differs greatly between cultures. Ingredients such as cheese, potatoes and various types of fish tend to be used a lot in Dutch dishes and meals, whereas German dishes do not.

##### Religion
The influence of religion on a culture is another thing that varies widely. Here in the Netherlands, while various religions are present within the country, none of them have a prominent role within the culture. This is in stark contrast to middle-east countries, where religion is woven deeply into the culture.

#### What is your culture?
In an attempt to find out more about my personality/culture, I completed one of [Myers Briggs' personality tests](https://www.16personalities.com/free-personality-test) and viewed the results. The results of this test would suggest that I have the [architect personality type](https://www.16personalities.com/intj-personality), which is known for valuing intellect and rationality above all else, their independence and their hate for following others without reason. While I do doubt whether I qualify for the supposed inginuity this personality type is supposed to possess, most of the personality traits (thirst for knowledge, distant/possibly offensive to strangers, prefering to rely on skill) described feel very familiar.

![PersonalityTestResults](https://user-images.githubusercontent.com/32673522/173200335-86a0ba73-e547-465a-bcaf-bd35f85e7834.png)

## Requirements and design
The goal of this learning outcome is to create, discuss and verify (functional) requirements and (architectural) designs.

### Requirements
At the start of the project, we created a list of user stories, which would act as our requirements. Each user story would be disected into smaller tasks, which would then get implemented individually. We created a primitive list of requirements for each task, so the assigned team members could see what needed to be done while the others could easily view the progress.

![Partial user story list](https://user-images.githubusercontent.com/32673522/171046329-f0008016-af64-4870-a9ff-cad96b96b944.png)

During the first period of our project, we made the fatal mistake of not asking enough about what the stakeholder truly wanted, and worked based on the assumption of what he wanted as explained in the project description. Because of this mistake, we ultimately had to scrap most of our progress partway through the development process and start anew with thorougly questioning the stakeholder on what the problem he faced is, and what it is that he seeks in order to solve it.

We then carefully filtered through the information we obtained from this meeting and created a first set of requirements that would help us get started with this second attempt (see [What Agile method did we use](#what-agile-method-did-we-use)). We would also increase the amount of meetings with the stakeholder to once per week, so we could show our progress, receive feedback and ask questions. This in turn significantly increased our understanding of what it is that we needed to create, as well as building a friendly relationship with the stakeholder. While we did not directly verify the list of requirements from our sprint board, we did verify whether or not we made the correct assumption and implemented the right thing during our meetings.

### Design
While I was personally not involved with the (architectural) design of the front-end application. Instead, my focus and input was made in the back-end application, where I discussed and implemented various choices based on past experiences, the needs of the project and the opinions of both my group members and the stakeholder.

One such design choice was **the implementation of the repository pattern**. During the early stages of the project, we worked with raw SQL queries, but quickly realized the poor maintainability and scalablilty of this approach. For this reason, I opted to implement an ORM to streamline the process and make the code shorter and more maintanable. However, I still wished to have the modularity of switching between different data storage mediums, should the need arise.

In order to facilitate this desire, I decided to implement the repository pattern. This addition, while still defining the base functions for each data type stored in the database, would allow the developer to impelemt functionality for a new ORM with as little as a single new base repository class. The accompanying interfaces also allowed us to implement dependency inversion, globalizing the entire system.

Another design choice I made in conjunction with another group member was **the best method to determine the sensor closest to a specific set of coordinates**. At first, the stakeholder wanted to have any sensors who did not send any new data to instead copy the measured temperature of the sensor closest to it. In order to achieve this, we devised two different methods.

The first method consisted of comparing the coordinates of each sensor to that of the one with no temperature reading, then taking the one whose coordinates come closest to those numbers. The second method consisted of mathematically calculating the distance between the each of the sensors and the sensor with no temperature reading and taking the sensor whose resulting value is the lowest. After discussing both methods and writing pieces of test code, we came to the conclusion that the second method was the most compact and maintainable.

However, the introduction of this functionality would ultimately cause an issue for which the solution was not part of the scope of our project. When presenting our findings to the stakeholder along with an alternative solution, he made the decision to cut the feature. While the code does not serve it's original purplose any longer, it ultimately got reused for determining the sensors closest to the corners of the grid, which are needed by the front-end heatmap generator.

## Business process
The goal of this learning outcome is to analyze and describe a business process related to the project.

### What is a business process?
A [business process](https://en.wikipedia.org/wiki/Business_process) is the flow of tasks that are required for a particular product or service to be provided to a customer. By making use of business models, one could potentially increase customer satisfaction and improve the ability to respond to (rapid) change. A popular manner of visualizing business processes is through the use of flow charts. These allow the viewer to get a clear image of the flow of events, as well as how unexpected developments are handled. Depending on the type of business process, the visibility of this process to the customer might vary widely. 

A business process starts with a goal, and finishes once the process reaces an objective that provides the customer with value, be this tangible or not. Business processes can globally be organized in one of three categories based on the role of the business process:
- **Core process**: A type of business process that is integral to a business' main value stream.
- **Management process**: A type of business process that oversees and manages (one of) the core processes.
- **Support process**: A type of process that fulfills a supportive role in (one of) the core processes.

Large/complex business processes can also be contain more specific/detailed sub-processes, which may be organized in one of the above categories as well.

While business processes are most commonly visualized as flow charts, the form may vary widely. One example would be the flow chart as depicted in the [Analysis](#analysis) section of this document, which resembles a flow chart as seen in software development. Another type of chart that could be used to vizualize business processes is a **swimlane diagram**. Unlike flow charts, these diagrams visualizes a business process as a pool, where each lane represents a facility/system/person. The advantage of using this model is the ability to see who is responsible for what, as each task/decision is placed in the lane of the one responsible for it's completion. However, the disadvantage is that, with large amounts of actors or a large-scaled business process, the model could become convoluted quickly.

![Swimlane diagram example](https://user-images.githubusercontent.com/32673522/173203603-072a994f-1523-43cd-af3f-0e718fad1986.png)

### Analysis
As neither our group project, nor my own individual project have a true business process, I decided to make an analisys of a more generic business process.
![Business process](https://media.discordapp.net/attachments/913358225724821534/978230657522204672/Picking.png?width=1440&height=208)

The above flow-chart depicts the business process of ordering a product from an online webshop. The goal of the business process is to prepare the items listed in the customer's order for shipment, and starts after the order has been succesfully placed. Once the order has been assigned to an employee, they will start collecting the products listed in the order and scan these in order to update the inventory system. For each product, the storage location will be provided. If a particular product cannot be found at the designated location, the employee will mark the product as missing and continue with the next item on the list. Once all items have been collected, the employee will deliver the products to the shipment section, where they will be prepared for further transport.

### How could this be used in other projects?
As mentioned before, our project did not have a true business process, partially due to it's small scale and use only being for employees of the company the applicatio was made for. Having this said however, I definitely see the value of visualizing business processes in larger projects, where one or multiple end-users/subsystems are involved. Having a clear image of how a process should flow from start to end, as well as the possible points of failure and how to handle them helps with understanding what systems, employees or services are required for the user to get something of value out of it.

I personally think that business processes are valuable pieces of information. It gives me as a software developer a clear view into the process, which steps the process goes through from start to end and the people involved in it, and it would certainly aid me in finding out what sort of software system(s) need to be built for the process to be completable, and I imagine the same is true for those in the business seats.

## Professional
The goal of this learning outcome is to work in a professional manner as part of a group in a software development.

### Communication & feedback
As described in the Agile section of this document, we used the SCRUM framework during this semester while working on the group projects. Our sprints were three weeks long, after which we would present our progress to the stakeholder, collect and write down feedback and plan ahead for the next sprint with the information collected from this meeting. We would also have a retrospective, in which we would look back at how the sprint went, and what we could possibly improve on to make the next sprint better. We would also individually give one another peer feedback.

![Peer feedback](https://user-images.githubusercontent.com/32673522/171042297-fd5116df-16b0-468b-aad7-c67f3eded8e1.png)

### Source control & code quality
During our project, we set up a GitHub repository where the source code for our project would be hosted. We created branches for different purposes: 
- The main branch, the location of the latest working build and responsible for building and publishing said build.
- The development branch, used for merging all work created by everyone in other branches and responsible for testing and analyzing the code.
- The function branches, based on the latest development branch and used to implement a new feature without creating merge conflicts by working in the same branch.

We used CI/CD to automate the analization/publishing process. In this particular project, SonarCloud was used to analyze the code quality.

![Group project SonarCloud scan results](https://user-images.githubusercontent.com/32673522/171047517-c4ed081c-1168-4ac1-be0a-da80da3111f9.png)

## Sources
[Agile Alliance on Agile](https://www.agilealliance.org/agile101/)

[ACM Code of Ethics](https://ethics.acm.org/code-of-ethics/software-engineering-code/)

[Trompenaars Model of Cultural Dimensions](https://expertprogrammanagement.com/2017/10/trompenaars-cultural-dimensions/)

[Explanation on Business process](https://en.wikipedia.org/wiki/Business_process)
