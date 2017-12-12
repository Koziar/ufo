### Useful links:
* [Blog Presentations](https://docs.google.com/spreadsheets/d/1n97YK_4i60fye4hHfsfY9Ugm_-m1qdWhmIumGjdJwVk/edit#gid=0)
* [UFO Plan](https://datsoftlyngby.github.io/soft2017fall/UFO_plan.html)

# Continuous Integration: with Travis and Jenkins

### Abstract

#### Creating applications that are continuously being improved by a team of developers can bring up a lot of unwanted problems. This problem has been around for a long time and has slowed down the progression of programs or applications that are continuously improved. Our solution to the problem is to automate the deployment process and continuously integrate new code using cutting edge tools. This has led to safer and faster development as functional and mergeable code is always delivered.

#### Continuous Integration

Continuous integration (CI) is the practice that supports developers in delivering better software in a more reliable and predictable way. It solves the problems that developers face while developing software and delivering it to end users.
In the traditional software development cycle, each developer gets a copy of the code from the central repository and start working on adding a new feature or fixing a bug. At the same time other developers work on their own task they have been assigned, changing the code. When one of them is done, he copies his code to the central repository and here two scenarios can take place at this point. If the code in the central repository is unchanged then things are simple, the new code can be delivered to the users without additional changes. The second scenario is when the code in the central repository has changed and the developer realises that at the point when he tries to copy his code into there. If there are merge conflicts then he needs to resolve them in order to be able to successfully deliver his piece of code and in this case things could get very complicated. If conflicts are related to modifying the same chunk of code by two developers then these are easy to resolve. However if for instance the API that developer was relying on have changed, the entire subsystem of the application behaves in a different way or other issues come up, the amount of work needed to solve a problem that was initially assigned to a developer can easily double. From minor changes to major structural changes. In serious situations a developer would have to carefully study the changes and almost certainly face partial or even a full rewrite of his solution. Many factors can escalate these problems, for example the size of the team working on the same project or the amount of time passed since the developer got his latest version of the source code from the central repository. Many people working on the same repository, over a long period of time, changing the context in which code needs to run can force a single developer to do a complete rewrite of his solution which is a tremendous waste of time and money. Besides that this traditional process has a number of other negative consequences for the business, like fixing and testing unexpected changes can take forever, releases are running late and the whole team gets stressed out because of upcoming deadlines and unpredictable release cycles

Continuous integration is the solution to all these problems. The big integration events need to be split into much smaller integrations. This way, all developers working on the same project need to deal with a much smaller number of changes, which are easier to understand and manage. To accomplish that, the iterations need to happen often, a couple times a day is ideal.

![Jenkins](https://github.com/Koziar/ufo/blob/master/jenkins.png)

Jenkins is a continuous integration server based on Java. Jenkins can be used in integration with many different tools and workflows. It is a highly customizable CI service that can be adapted for almost any use case imaginable. With a large user base and a huge amount of plugins. In order to run Jenkins a server specifically dedicated to running a Jenkins instance has to be deployed. In order to configure a very basic Jenkins workflow all you would have to download the needed plugins and configure each one of them with the needed credentials and the proper workflow. A simple Jenkins set-up could seem simple but there is a lot more that could go wrong than a Travis set-up. That being because Travis is made to complete a CI chain always using GitHub meaning that every Travis setup will have the same structure, unlike Jenkins which can have a highly customized CI chain with conditionals and many other things.

![Travis CI](https://workablehr.s3.amazonaws.com/uploads/account/logo/11901/large_Mascot-fullcolor-png.png)

Travis is a hosted service which focuses on continuously integrating github projects. Travis builds and tests projects and eventually deploys the project the specified host. Travis is very easy to set-up as it is very well integrated with GitHub. Any project hosted, open-source or not, on GitHub can easily set-up a CI chain using Travis. Travis solves a handful of problems easily and efficiently but when it comes to full customization and edge cases Travis could fall short-handed. This being the reason as to why we decided to use Travis for our front-end project as we knew it would not need any complex configuration. Another advantage with Travis is that it is an already hosted service so unlike Jenkins you would not have to deploy Travis on its own dedicated server.

This is done through a yml file in the project root folder, the travis.yml file. Within this file all of the deployment configuration is set as well as all commands that are wanted to be run before the project is deployed to the host. In addition to a yml file Travis needs to be configured in GitHub, where there is a very intuitive user interface to guide you through the process of setting your Travis CI.


#### Which to use when?

It is always hard to decide which tool or service to use in any project. If you are looking for something that is easy to get started with and does not require a large amount of configuration Travis CI is definitely the way to go. Although you have to remember that if you do want to use Travis CI you will also have to use GitHub for code versioning. Travis CI also has a lot of pre created configuration files online that can be used to start your Travis experience even without really knowing what you are doing. 

On the other hand Jenkins is a highly configurable CI service that can be used with nearly any tool and has a huge library of plugins that make Jenkins compatible with almost any tool imaginable. If you are not using GitHub Jenkins is an obvious go to also if extensive customization is needed. Jenkins can have conditional build and deploys and many other features. Also unlike Travis you do not have to pay in order to have it used in private projects. But that cost could be made up in having to host Jenkins in a dedicated server, which could also lead having to have a developer taking care of the server.

All in all both services have their advantage and are great services for CI.

##### Refs:
* https://docs.travis-ci.com/
* https://jenkins.io/doc/
