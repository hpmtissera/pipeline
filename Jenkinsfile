pipeline {
    agent any

    tools {
        jdk 'Java 8'
        maven 'Maven'
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
                   export branch_name=$env.BRANCH_NAME
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
