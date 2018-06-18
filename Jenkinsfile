def jobnameparts = JOB_NAME.tokenize('/') as String[]
def jobconsolename = jobnameparts[0]
pipeline {

    agent {
        echo "${jobconsolename}"
        any
        customWorkspace '$GIT_BRANCH'
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
                   export branch_name=$GIT_BRANCH
                   ./Build/ova-build.sh
                '''
               
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'target/*'
        }
    }
}
