pipeline {
    agent any
    tools {
        nodejs 'NodeJS'
    }
    environment {
        NEXUS_URL = 'http://localhost:8081/repository/npm-hosted/'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/dhurbab27/hello-world-react-package'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat """
                    npm install
                """
            }
        }
        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }
        stage('Publish') {
            steps {
                withCredentials([string(credentialsId: 'nexus-auth-token', variable: 'NPM_AUTH_TOKEN')]) {
                    bat """
                        call npm set registry http://localhost:8081/repository/npm-hosted/
                        call npm set //localhost:8081/repository/npm-hosted/:_authToken=%NPM_AUTH_TOKEN%
                        call npm publish --loglevel verbose
                    """
                }
            }
        }

    }
}
