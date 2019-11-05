pipeline {
    agent {
        docker {
            image 'ubuntu:dockerfile'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true' 
    }
    stages {
        stage('Build') {
            steps {
                sh 'python --version'
                sh 'yarn global add gatsby-cli'
                sh 'yarn install'
            }
        }
        stage('Deploy') { 
            steps {
                sh 'yarn run deploy' 
            }
        }
    }
}
