import hudson.tasks.test.AbstractTestResultAction
def jobnameparts = JOB_NAME.tokenize('/') as String[]
def jobconsolename = jobnameparts[0]
def branchname = jobnameparts[1]
pipeline {

    agent {
        node {
            label 'master'
            customWorkspace "${jobconsolename}"
        }
    }

    environment {
        jobconsolenameshell = "${jobconsolename}"
    }

    tools {
        jdk 'Java 8'
        maven 'Maven'
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '2'))
    }

    stages {
        stage('Example Build') {
            steps {
                sh 'echo $JAVA_HOME'
                sh 'mvn clean install'
            }
        }
        stage('Example Test') {
            steps {
                echo 'Testing'
                echo 'Changes introduced 1234'
            }
        }
        stage('Build pipleline sub') {
            steps {
                dir('pipeline-sub') {
                    git branch: 'mybranch',
                            url: 'https://github.com/prasadlvi/pipeline-sub.git'
                    sh '''
                       cat README.md
                      '''
                }
                sh '''
                   export prev_version='My previous version'
                   cd ..
                   export branch_name=${PWD##*/}
                   cd -
                   ./Build/test-script.sh
                '''
               
            }
        }
    }

    post {
        always {
            
            script {
               try {
                  junit 'target/surefire-reports/TEST-*.xml'
                  archiveArtifacts artifacts: 'target/*'
               } catch (Exception e) {
                  currentBuild.result = 'FAILURE'
               }
            }

            script {
                if (currentBuild.result == null) {
                    currentBuild.result = 'SUCCESS'
                } else if(currentBuild.result == 'FAILURE' || currentBuild.result == 'UNSTABLE') {
                    emailext to: 'test@test.com test1@test.com', subject: '$DEFAULT_SUBJECT', body: '$DEFAULT_CONTENT'
                    if(currentBuild.result == 'FAILURE') {
                        mattermostColor = "danger"
                        mattermostResult = "Failure"
                        mattermostEmoji = ":no_entry_sign:"
                    } else {
                        mattermostColor = "warning"
                        mattermostResult = "Unstable"
                        mattermostEmoji = ":warning:"
                    }
                }
                def testResultAction = currentBuild.rawBuild.getAction(AbstractTestResultAction.class)
                if (testResultAction != null) {
                    def total = testResultAction.getTotalCount()
                    def failed = testResultAction.getFailCount()
                    def skipped = testResultAction.getSkipCount()

                    summary = "\nTest results:\n\t"
                    summary = summary + ("Passed: " + (total - failed - skipped))
                    summary = summary + (", Failed: " + failed)
                    summary = summary + (", Skipped: " + skipped)
                } else {
                    summary = "No tests found"
                }
            }

            mattermostSend message: "${env.JOB_NAME} - #${env.BUILD_NUMBER} after ${currentBuild.durationString.replace(' and counting', '')} <${env.BUILD_URL}|Open>${summary}"
            echo "${summary}"
            sh "echo \$jobconsolenameshell"
            sh '''
             echo 'inside shell script'
             echo \$jobconsolenameshell
            '''

            echo "$branchname"

            echo "RESULT: ${currentBuild.result}"
            echo "Duration : ${currentBuild.durationString.replace(' and counting', '')}"

        }
    }

}
