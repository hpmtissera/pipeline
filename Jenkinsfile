def jobnameparts = JOB_NAME.tokenize('/') as String[]
def jobconsolename = jobnameparts[0]
pipeline {

    agent {
        node {
            label 'master'
            customWorkspace "${jobconsolename}"
        }
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
                   ./Build/ova-build.sh
                '''
               
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/*'

            script {
                if (currentBuild.result == null) {
                    currentBuild.result = 'SUCCESS'
                } else if(currentBuild.result == 'FAILURE' || currentBuild.result == 'UNSTABLE') {
                    emailext to: 'prasad@lvi.co.jp', subject: '$DEFAULT_SUBJECT', body: '$DEFAULT_CONTENT'
                }
            }

            sh '''
               echo ${currentBuild.result}
               '''
        }
    }
}
