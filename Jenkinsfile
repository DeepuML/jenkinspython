pipeline {
    agent any

    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/yourusername/my-python-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-python-app .'
            }
        }

        stage('Run Unit Tests') {
            steps {
                sh 'docker run --rm my-python-app pytest tests/'
            }
        }

        stage('Push Image to Local Registry') {
            steps {
                sh 'docker tag my-python-app localhost:5000/my-python-app'
                sh 'docker push localhost:5000/my-python-app'
            }
        }

        stage('Deploy to Docker Swarm') {
            steps {
                sh '''
                    docker stack deploy -c docker-compose.yml mystack
                '''
            }
        }
    }
}
    post {
        always {
            archiveArtifacts artifacts: '**/test-results.xml', allowEmptyArchive: true
            junit 'test-results.xml'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }

// This Jenkinsfile defines a CI/CD pipeline for a Python application using Docker and Docker Swarm.