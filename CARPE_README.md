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

## Building and Publishing

Because this is such a one-off emergency band-aid kind of thing, we are willing to just build this locally and push it up to S3.

When in S3, our Jenkins AMI build process will pull it down and then install our customized plugin into the Jenkins image.

To build this, run the following:
```bash
git clone git@github.com:carpe/devops-sonar-scanner-jenkins.git
cd devops-sonar-scanner-jenkins
git checkout  DOPS-3438-modify-the-sonar-jenkins-plugin
./mvnw clean install -Dmaven.test.skip=true
```
The compile may take a while.

This will result in a file called `sonar.hpi` in the `target` directory.
This is the file we will publish to S3.
```bash
VERSION="2.15.6"
aws s3 cp target/sonar.hpi s3://carpe-jenkins/plugins/${VERSION}/sonar.hpi
```

After publishing the plugin, you will need to go to the Jenkins project and [change the location in S3 that the plugin is being pulled from](https://github.com/carpe/devops-jenkins/blob/master/ami-creation/jenkins-controller/files/jenkins-startup.sh) to match the version that you just published.
You can then create a new AMI to make sure that the next Jenkins you deploy will have this plugin in it.

## Testing

To test whether the plugin works without actually publishing it and rebuilding an AMI, you can create a new green Jenkins.
After creating the green Jenkins, you can upload this plugin manually using the Deploy Plugin feature in the [Plugin Advanced Settings page](https://jenkins-controller-green.ss.carpe.io/manage/pluginManager/advanced).
You may need to restart Jenkins for it to take effect. This can be done on [the Restart page](https://jenkins-controller-green.ss.carpe.io/restart).
