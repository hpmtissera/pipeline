pipeline {
    agent any

    environment {
        JAVA_HOME='/Library/Java/JavaVirtualMachines/jdk1.8.0_171.jdk'
    }

    tools {
        jdk 'Java 8'
        maven 'Maven'
    }

    stages {
        stage('Example Build') {
            steps {
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
                sh './Build/ova-build.sh'
            }
        }
    }
}
