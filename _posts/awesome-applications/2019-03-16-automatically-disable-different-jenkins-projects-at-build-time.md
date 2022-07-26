---
categories:
  - awesome-applications
  - jenkins
tags:
  - jenkins
layout: post
title: Automatically Disable Different Jenkins Projects at Build Time
created: 1552713446
---

I use Jenkins as my CI tool for all my personal projects.  My current Jenkins build plans are fairly simple and not quite particularly complex (though I do plan on eventually start using Jenkins pipelines on my build jobs in the near future), given that most of my personal projects are WordPress and Drupal sites.

My current configuration consists of two different basic Freestyle projects Jenkins builds. One for my staging build/job and the other for my production build/job respectively. Each time my staging Freestyle project builds, it automatically creates a git tag, which is later used by my production Freestyle project; where it's pulled, build, and deploy from. This means that at no point I want my production Freestyle project to build whenever the corresponding staging Freestyle project fails (for example unit tests).

Using the Groovy <a href="https://wiki.jenkins.io/display/JENKINS/Groovy+Postbuild+Plugin" target="_blank">Postbuild Plugin</a>, will give you the ability to modify Jenkins itself. In my case, I want to disable the productions Freestyle project whenever my staging Freestyle project.

On this example my production project build/job is called <strong>rubysecurity.org</strong>. 
```java
import jenkins.*
import jenkins.model.*

String production_project = "rubysecurity.org";

try {
  if (manager.build.result.isWorseThan(hudson.model.Result.SUCCESS)) {
    Jenkins.instance.getItem(production_project).disable()
    manager.listener.logger.println("Disabled ${production_project} build plan!");
    manager.createSummary("warning.gif").appendText("<h1>No production builds will be available on ${production_project} until the errors here are fixed!</h1>", false, false, false, "red")

  } else {
    Jenkins.instance.getItem(production_project).enable();
    manager.listener.logger.println("Enabled ${production_project} build plan.");
  }
} catch (Exception ex) {
   manager.listener.logger.println("Error disabling ${production_project}." + ex.getMessage());
}
```

The example Groovy Post-Build script ensures the project build/job `rubysecurity.org` is enabled if it successfully finishes without any errors, otherwise `rubysecurity.org` will be disabled, and a custom error message is displayed on the failing staging build/job.

Example error:

<img src="/assets/awesome-applications/jenkins-error.png" alt="Jenkins error">

References:

* <a href="https://stackoverflow.com/questions/8661349/disable-jenkins-job-from-another-job" target="_blank">https://stackoverflow.com/questions/8661349/disable-jenkins-job-from-another-job</a>
