pipeline {
    agent any

    stages {
        stage ('cleaning workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Cloning The Code') {
            steps {
                echo 'Cloning code from GitHub'
                git branch: 'main', url: 'https://github.com/Shahid0099/Flask-app.git'
            }
        }
        stage('build') {
            steps {
                echo 'Buildind code'
                sh "docker build -t shadow493/flask-app:latest ."
            }
        }
        stage('Push to DockerHub') {
            steps {
                echo 'Pushing Image to Hub'
                withDockerRegistry(credentialsId: 'dock-cred', url: 'https://index.docker.io/v1/') {
                sh 'docker tag flask-app shadow493/flask-app:latest'
                sh 'docker login'    
                sh 'docker push shadow493/flask-app:latest'
                }
            }
        }
        stage('Delpoy App') {
            steps {
                echo 'Deploying With Docker Compose'
                sh 'docker compose down --remove-orphans || true'
                sh 'docker compose up -d --build'
            }
        }
        stage('Cleanup') {
            steps {
                sh '''
                docker container prune -f
                docker image prune -f
                '''
            }
        }
    }
}
