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
                dir('pipeline-sub') {
                   git branch: 'mybranch',
                       url: 'https://github.com/prasadlvi/pipeline-sub.git'
                }
                sh '''
                    cd pipeline-sub
                    cat README.md
                '''
            }
        }
    }
}
