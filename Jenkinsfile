pipeline {
    agent { label 'host1-yoga' }

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
                -Dsonar.host.url=http://172.23.4.114:9000 \
                -Dsonar.token=sqp_b6b028ca74c39bf4c64a374119a4bf8087b98ec6
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