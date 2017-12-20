### Useful links for us:
* [Blog Presentations](https://docs.google.com/spreadsheets/d/1n97YK_4i60fye4hHfsfY9Ugm_-m1qdWhmIumGjdJwVk/edit#gid=0)
* [UFO Plan](https://datsoftlyngby.github.io/soft2017fall/UFO_plan.html)
* [Blog Entry to review](https://github.com/ovi28/UFO)
# Continuous Integration: with Travis and Jenkins
(Edited after the review)
### Abstract

#### Creating software that is continuously being improved by a team of developers can make it hard to have the newest functional code in a production environment. This problem has been around for a long time and has slowed down the development and release cycles. Our solution to the problem is to automate the deployment process and continuously integrate new code using cutting edge tools. This has led to safer and faster development as functional code is always delivered and developer hours are not wasted.


#### Continuous Integration

Continuous integration (CI) is the practice that supports developers in delivering better software in a more reliable and predictable way by integrating their work frequently. The idea is that each developer in a team integrates their code at least daily, so within the team it leads to multiple integration per day. It solves the problems that developers face while developing software and delivering it to end users. (https://martinfowler.com/articles/continuousIntegration.html)

In the traditional software development cycle, each developer gets a copy of the code from the central repository and start working on adding a new feature or fixing a bug.
At the same time other developers work on their own task they have been assigned, changing the code. When one of them is done, he copies his code to the central repository and here two scenarios can take place at this point.

If the code in the central repository is unchanged then things are simple, the new code can be delivered to the users without additional changes.

The second scenario is when the code in the central repository has changed and the developer realises that at the point when he tries to copy his code into there. If there are merge conflicts then he needs to resolve them in order to be able to successfully deliver his piece of code and in this case things could get very complicated.
If conflicts are related to modifying the same chunk of code by two developers then these are easy to resolve.
However if for instance the API that developer was relying on have changed, the entire subsystem of the application behaves in a different way or other issues come up, the amount of work needed to solve a problem that was initially assigned to a developer can easily double. From minor changes to major structural changes. In serious situations a developer would have to carefully study the changes and almost certainly face partial or even a full rewrite of his solution.
Many factors can escalate these problems, for example the size of the team working on the same project or the amount of time passed since the developer got his latest version of the source code from the central repository. Many people working on the same repository, over a long period of time, changing the context in which code needs to run can force a single developer to do a complete rewrite of his solution which is a tremendous waste of time and money.

Besides that this traditional process has a number of other negative consequences for the business, like fixing and testing unexpected changes can take forever, releases are running late and the whole team gets stressed out because of upcoming deadlines and unpredictable release cycles.

Continuous integration is the solution to all these problems. The big integration events need to be split into much smaller integrations. This way, all developers working on the same project need to deal with a much smaller number of changes, which are easier to understand and manage. To accomplish that, the iterations need to happen often, a couple times a day is ideal.

![Jenkins](https://github.com/Koziar/ufo/blob/master/jenkins.png)

Jenkins is a continuous integration server based on Java. Jenkins can be used in integration with many different tools and workflows. It is a highly customizable CI service that can be adapted for almost any use case imaginable. With a large user base and a huge amount of plugins. In order to run Jenkins a server specifically dedicated to running a Jenkins instance has to be deployed. In order to configure a very basic Jenkins workflow all you would have to download the needed plugins and configure each one of them with the needed credentials and the proper workflow. A simple Jenkins set-up could seem simple but there is a lot more that could go wrong than a Travis set-up. That being because Travis is made to complete a CI chain always using GitHub meaning that every Travis setup will have the same structure, unlike Jenkins which can have a highly customized CI chain with conditionals and many other things.
(https://jenkins.io/doc/)

![Travis CI](https://workablehr.s3.amazonaws.com/uploads/account/logo/11901/large_Mascot-fullcolor-png.png)

Travis is a hosted service which focuses on continuously integrating github projects. Travis builds and tests projects and eventually deploys the project the specified host. Travis is very easy to set-up as it is very well integrated with GitHub. Any project hosted, open-source or not, on GitHub can easily set-up a CI chain using Travis. Travis solves a handful of problems easily and efficiently but when it comes to full customization and edge cases Travis could fall short-handed. (https://docs.travis-ci.com/user/for-beginners)
This being the reason as to why we decided to use Travis for our front-end project as we knew it would not need any complex configuration. Another advantage with Travis is that it is an already hosted service so unlike Jenkins you would not have to deploy Travis on its own dedicated server.

#### What we did

In order to be able to accurately compare both services we decided to implement a CI chain using both Jenkins and Travis. From this we were able to gather our own opinion of both. In this section we will not show exactly how to setup either Jenkins or Travis but rather give you an overview of how you would do it and all of the components needed in order to do so.

For Jenkins we set up a basic CI chain that would integrate our local source code once it had been pushed to GitHub to our production Digital Ocean Droplet. It basically accomplishes the exact same thing as a Travis CI chain would accomplish. In order to set-up the CI chain we had to firstly create a GitHub repo for the source code, set-up our Jenkins server, set-up a Docker Hub repo and have a Digital Ocean Droplet for hosting the Server. Another requirement for this CI chain was that our Droplet would have to be Dockerized as we use Docker Hub to host our Docker images and then fetch the image from the server. (https://github.com/datsoftlyngby/soft2017fall-lsd-teaching-material/blob/master/lecture_notes/05-Continuous%20Integration%20and%20Delivery.ipynb)

The diagram below illustrates the setupset-up we used for our Jenkins CI chain. Each block is a separate service/instance. Each arrow starts from the block which initiates contact to the block the arrow is pointing to. The numbers on the arrows describe the order in which the actions happen. From this diagram you can see exactly how we managed to set-up our Jenkins CI chain.

![Jenkins CI chain flow](https://github.com/Koziar/ufo/blob/master/Screen%20Shot%202017-12-20%20at%2013.39.06.png)
*Jenkins CI chain flow.

As can be seen in the diagram we have not deployed Jenkins onto its own server. This is due to financial limitations. We as a team did not think it was worth investing more money into this process and decided to sacrifice fully automated CI. But, if our Jenkins had been deployed onto a server that would then have created a fully autonomous CI chain.

Second CI chain that we have setup for our front-end part of the Hacker News project was based on Travis deploying to AWS S3 bucket. To integrate it into our development workflow, first we had to add the project into Travis service by flicking the repository switch on, that is being done from the user profile settings on Travis. You basically choose to which of your repositories you allow Travis to have access to. After this step we have added .travis.yml file into our repository and configured it accordingly so it knows how to install dependencies from npm, build the project and where to deploy it. In the deployment part of the .travis.yml file we had to assign specific environment variables like AWS access key, access secret, bucket name and the region. Those variables are being defined in the settings of that project on Travis. With that setup, every successful build of a merged pull request is being deployed to S3 bucket and goes to the production. A diagram that we made to show off the described chain can be found below. At first glance you can already see it is much simpler and easy to setup, configure and use right away you touch it, comparing to Jenkins, which requires much more adjustment. 

![Travis CI chain flow](https://github.com/Koziar/ufo/blob/master/Screen%20Shot%202017-12-20%20at%2013.39.20.png)
*Travis CI chain flow.

#### Technical Comparison

This comparison is made in terms of setting up our use case. In which Travis and Jenkins are very much comparable. For certain use cases this comparison may not apply. 

Initial setup: Travis > Jenkins
Speed: Travis === Jenkins
Interface: Travis > Jenkins
Customizability: Travis < Jenkins
Price: Travis (cheaper) < Jenkins

* This is due to us having to deploy Jenkins and paying for server space. In other cases Travis prices (https://travis-ci.com/plans) and server hosting space (for Jenkins) are the way to compare. 

#### So which one should you use?

It is always hard to decide which tool or service to use in any project. If you are looking for something that is easy to get started with and does not require a large amount of configuration Travis CI is definitely the way to go. Although you have to remember that if you do want to use Travis CI you will also have to use GitHub for code versioning. Travis CI also has a lot of pre created configuration files online that can be used to start your Travis experience even without really knowing what you are doing. 

On the other hand Jenkins is a highly configurable CI service that can be used with nearly any tool and has a huge library of plugins that make Jenkins compatible with almost any tool imaginable. If you are not using GitHub Jenkins is an obvious go to also if extensive customization is needed. Jenkins can have conditional build and deploys and many other features. Also unlike Travis you do not have to pay in order to have it used in private projects. But that cost could be made up in having to host Jenkins in a dedicated server, which could also lead having to have a developer taking care of the server.

If neither Travis or Jenkins befit your use case there are many alternative such as:
* Shippable (http://www.shippable.com)
* CircleCI (https://circleci.com)
* CodeShip (https://www.codeship.com)
These are some popular alternatives that also have large communities backing them. For more alternatives look at: https://stackify.com/top-continuous-integration-tools/

##### Sources:
* https://martinfowler.com/articles/continuousIntegration.html
* https://www.thoughtworks.com/continuous-integration
* https://docs.travis-ci.com/user/for-beginners
* https://docs.travis-ci.com/user/getting-started/
* https://docs.travis-ci.com/user/customizing-the-build/
* https://github.com/datsoftlyngby/soft2017fall-lsd-teaching-material/blob/master/lecture_notes/05-Continuous%20Integration%20and%20Delivery.ipynb
* https://jenkins.io/doc/
* https://travis-ci.com/plans
* http://www.shippable.com
* https://circleci.com
* https://www.codeship.com
* https://stackify.com/top-continuous-integration-tools/
