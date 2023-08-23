# DEVOPS' Sonarcloud Plugin README

## Background
Our Jenkins UI is getting locked on multi-branch pipelines after some indeterminent abount of time.  The assumption is we’re getting throttled by a non-authenticated call to the API https://sonarcloud/api/server/version.  We want to try modifying the plugin to hard-code this return value from this call, thus eliminating this throttling.

## Acceptance Criteria
1. fork the repo to Carpe
1. clone locally
1. create branch `DOPS-3438-modify-the-sonar-jenkins-plugin` from Tag `sonar-2.15`
1. modify the getServerVersion call to return “8.0.0.44909“
1. modify the POM to show version 2.15.4
1. test on Green Jenkins
1. deploy to SDLC Jenkins

## Building
```bash
$ git clone git@github.com:carpe/devops-sonar-scanner-jenkins.git
$ cd devops-sonar-scanner-jenkins
$ git checkout  DOPS-3438-modify-the-sonar-jenkins-plugin
$ ./mvnw clean install -Dmaven.test.skip=true
```