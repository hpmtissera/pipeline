pipeline {
    agent any
    stages {
        stage('Example Build') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Example Test') {
            steps {
                echo 'Testing'
            }
        }
        stage('Build pipleline sub') {
            steps {
                sh 'pwd'
                dir('pipeline-sub') {
                    sh 'pwd'
                    git branch: 'mybranch',
                            url: 'https://github.com/prasadlvi/pipeline-sub.git'
                    sh '''
                       pwd
                       cat README.md
                      '''
                }
                sh './Build/ova-build.sh'
            }
        }
    }
}
