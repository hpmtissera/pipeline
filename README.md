# Jenkins Pipeline Sample

This sample project demonstrate how to write a Jenkins pipeline script. It cover the following scenarios.

- Checkout the project to a custom workspace.  
- Build the project with Maven, run tests.  
- Checkout pipeline-sub into a new folder inside the workspace.  
- Export some variables using a block shell script.  
- Run another shell script that uses the values of exported variables.  
- Publish JUnit test results.  
- Publish project artifacts.  
- Send email notification if the build is a Failure, Unstable or Back to Normal. (No emails for back to back successful builds)  
- Send Mattermost notification with build time, build URL and test results summary.  
