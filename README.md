# Jenkins

![Jenkins Logo](https://raw.githubusercontent.com/powerhome/phrg-jenkins/master/Jenkins-Logo.png?raw=true "Jenkins Logo")

[Jenkins](https://github.com/jenkinsci/jenkins) is an open source continuous integration, continuous deployment software that can be used to automate building, testing, and deploying software. Power uses it for all of these purposes, and it enables us to update Nitro continuously throughout the day.

## Continuous Integration

Continuous Integration (CI) is the process of automating the building and testing of code every time a team member commits changes to version control.

As defined by [Sten Pittet of the Atlassian](https://www.atlassian.com/continuous-delivery/ci-vs-ci-vs-cd):

"Developers practicing continuous integration merge their changes back to the main branch as often as possible. The developer's changes are validated by creating a build and running automated tests against the build. By doing so, you avoid the integration hell that usually happens when people wait for release day to merge their changes into the release branch.

Continuous integration puts a great emphasis on testing automation to check that the application is not broken whenever new commits are integrated into the main branch."

Read about [continuous integration on Wikipedia](https://en.wikipedia.org/wiki/Continuous_integration).

## Continuous Deployment

Continuous Deployment (CD) takes this process one step further. [Sten Pittet continues](https://www.atlassian.com/continuous-delivery/ci-vs-ci-vs-cd):

"...With this practice, every change that passes all stages of your production pipeline is released to your customers. There's no human intervention, and only a failed test will prevent a new change to be deployed to production.

Continuous deployment is an excellent way to accelerate the feedback loop with your customers and take pressure off the team as there isn't a Release Day anymore. Developers can focus on building software, and they see their work go live minutes after they've finished working on it."

## Nitro

At Power, it is common to refer to Jenkins as our "CI", but "CI/CD" is a more complete description. Moving forward, these lessons will refer to Jenkins simply as Nitro's CI.

Jenkins is integrated with Nitro through [Github Webhooks](https://help.github.com/articles/about-webhooks/), just like Codeclimate. Anytime one adds a commit to a pull request on the `nitro-web` repo, it triggers a CI run. Once Jenkins has finished, it displays the result of the run right in Github:

![Successful CI Run](https://raw.githubusercontent.com/powerhome/phrg-jenkins/master/Green-CI-Run.png?raw=true "Successful CI Run")

### What does Jenkins do for Nitro?

For a new commit on a `nitro-web` Pull Request, Jenkins will initialize Nitro's environment, install dependencies, build & test the application, and then remove all the extra files and configuration created to make the early steps execute. In the old Jenkins UI, that looks like this:

![PR Build](https://raw.githubusercontent.com/powerhome/phrg-jenkins/master/PR-9136-Old-Jenkins-UI.png?raw=true "PR Build")

In the screenshot above, `Env Init`, `Dependencies`, `Build`, `Deploy` and `Cleanup` are called CI stages. For a **PR** build, the `Deploy` stage is skipped over. Only builds on `nitro-web`'s master branch will automatically deploy.

### Digging deeper with Blue Ocean

Just looking at the stages themselves does not reveal much about what is going on. Happily, Jenkins has a plugin called [Blue Ocean](https://jenkins.io/projects/blueocean/) that renders a more communicative interface. As you might guess, most of the views in Blue Ocean are blue :)

![Blue Ocean UI](https://raw.githubusercontent.com/powerhome/phrg-jenkins/master/Blue-Ocean-UI.png?raw=true "Blue Ocean UI")

In Blue Ocean, the same PR build will looks like this:

![Blue Ocean PR Build](https://raw.githubusercontent.com/powerhome/phrg-jenkins/master/Build-View-in-Blue-Ocean.png?raw=true "Blue Ocean PR Build")

Here, one can see that the `Deploy` stage is skipped because the corresponding node is empty. One can also see many of the individual processes that happen during the `Dependencies` and `Build` stages. If you click on a green node and scroll down to the bottom of the page, you will have access to another break down of the data. Here is what clicking on the "Overcommit" node displays.

![Blue Ocean Bottom of PR Build](https://raw.githubusercontent.com/powerhome/phrg-jenkins/master/Overcommit-Overview-in-Blue-Ocean.png?raw=true "Blue Ocean Bottom of PR Build")

The "Overcommit" process is broken down into four sections of logs. On the far right of each line, you can see how long each section took. Reveiwing that image above shows that most of the work happened in a section simply titled "false". Clicking on that line of text will reveal even more info.

![Blue Ocean Overcommit Logs](https://raw.githubusercontent.com/powerhome/phrg-jenkins/master/Overcommit-Logs.png?raw=true "Blue Ocean Overcommit Logs")

### The complete picture

To get a full scoop on how Jenkins works for Nitro, our ["Jenkin's Pipeline"](https://jenkins.io/doc/book/pipeline/) is defined in the `Jenkinsfile` located on the root of `nitro-web`. This file is written in [Groovy](http://groovy-lang.org/), a Java-syntax-compatible object-oriented programming language that was modeled off of features in Ruby. (There is even a [Groovy on Grails](https://grails.org/) Java framework that models `Ruby on Rails`).

## Resources

- [Article on CI & CD](https://www.atlassian.com/continuous-delivery/ci-vs-ci-vs-cd)
- [Jenkins Docs](https://jenkins.io/doc/)
