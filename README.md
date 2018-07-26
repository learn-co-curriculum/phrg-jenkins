# Jenkins

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

Jenkins is integrated with Nitro through [Github Webhooks](https://help.github.com/articles/about-webhooks/), just like Codeclimate. Anytime one adds a commit to a pull request against the `nitro-web` repo, it triggers a CI run. Once Jenkins has finished, it displays the result of the run right in Github:

![Successful CI Run](https://raw.githubusercontent.com/powerhome/phrg-jenkins/master/Green-CI-Run.png?raw=true "Successful CI Run")

### So what does Jenkins do for us?

For a new commit on a `nitro-web` Pull Request, Jenkins will initialize Nitro's environment, install dependencies, build & test the application, and then remove all the extra files and configuration created to make the early steps execute. In the old Jenkins UI, that looks like this:

![PR Build](https://raw.githubusercontent.com/powerhome/phrg-jenkins/master/PR-9136-Old-Jenkins-UI.png?raw=true "PR Build")




wip...

As Nitro grows, the answer to the question can vary. To get a full picture of our [Jenkin's pipeline](https://jenkins.io/doc/book/pipeline/), check out the `Jenkinsfile` located on the root of `nitro-web`. This file is written in [Groovy](http://groovy-lang.org/), a Java-syntax-compatible object-oriented programming language that was modeled off of features in Ruby. (There is even a [Groovy on Grails](https://grails.org/) Java framework that models `Ruby on Rails`).
