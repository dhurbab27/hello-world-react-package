pipeline {
    agent any
    tools {
        nodejs 'NodeJS'
    }
    environment {
        NEXUS_URL = 'http://localhost:8081/repository/npm-hosted/'
        NPM_AUTH_TOKEN = credentials('nexus-auth-token')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/dhurbab27/hello-world-react-package'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }
        stage('Publish') {
            steps {
              withCredentials([usernamePassword(credentialsId: 'nexus-creds', usernameVariable: 'NEXUS_USERNAME', passwordVariable: 'NEXUS_PASSWORD')]) {
                bat """
                    npm set registry $NEXUS_URL
                    npm set //$NEXUS_URL:username=$NEXUS_USERNAME
                    npm set //$NEXUS_URL:_password=$(echo -n $NEXUS_PASSWORD | base64)
                    npm publish
                """
                }

            }
        }
    }
}
