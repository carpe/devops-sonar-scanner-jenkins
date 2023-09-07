# DEVOPS' Sonarcloud Plugin README

## Background
Our Jenkins UI is getting locked on multi-branch pipelines after some indeterminent abount of time.  The assumption is we’re getting throttled by a non-authenticated call to the API https://sonarcloud/api/server/version.  We want to try modifying the plugin to hard-code this return value from this call, thus eliminating this throttling.

Apply the patch from this issue:
https://issues.jenkins.io/browse/JENKINS-70694?focusedId=441129&page=com.atlassian.jira.plugin.system.issuetabpanels%3Acomment-tabpanel#comment-441129

## Building
```bash
$ git clone git@github.com:carpe/devops-sonar-scanner-jenkins.git
$ cd devops-sonar-scanner-jenkins
$ git checkout sonar-2.15-thorsten-patch
$ ./mvnw clean install
$ # the plugin will be in target/sonar.hpi
```