pipeline {
    agent any
    

    stages {
        stage('Checkout SCM') {
            steps {
                git branch: 'dev', url: 'https://github.com/Yamin2810/simple-apps.git'

            }
        }
        stage('Build') {
            steps {
                sh '''cd apps
                npm install'''
            }
        }
        stage('Testing') {
            steps {
                sh '''cd apps
                npm test
                npm run test:coverage'''
            }
        }
        stage('Code Review') {
            steps {
                sh '''cd apps
                sonar-scanner \
                -Dsonar.projectKey=Test-Apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.4.112:9000 \
                -Dsonar.login=sqp_453c0e4301afd70ecdf1719a4e66bd5e2ceb78c6'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }
    }

     container('sonar') {
         stage('SonarQube Analysis') {
             steps {
                 sh '''cd apps
                 sonar-scanner \
                 -Dsonar.projectKey=sonartest \
                 -Dsonar.sources=. \
                 -Dsonar.host.url=http://172.23.4.112:9000 \
                 -Dsonar.login=sqp_c003baa63ac4fb48b9626952f073278756398982'''
             }
         }
     }
}
