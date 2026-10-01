pipeline {
    agent { label 'host1-yoga' }
    environment {
        SONAR_TOKEN = credentials('token-sonar')
        HOST_TOKEN = credentials('host-sonar')
    }
    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Ynfinteractive123/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'''
                cd apps
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd apps
                npm test
                npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd apps
                sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=${HOST_TOKEN} \
                -Dsonar.token=${SONAR_TOKEN}
                '''
            }
        }
        
        stage('Deliver') {
            steps {
              input message: 'Apakah anda sudah yakin untuk deploy ke production', ok: 'Deploy Sekarang!'
            }
        }

        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        
        
    }
}