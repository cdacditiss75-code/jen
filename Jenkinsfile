pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using the Dockerfile in the workspace
                    docker.build("my-nginx-app:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Stop and Remove Existing Container') {
            steps {
                // This is highly dependent on your environment.
                // Example to stop and remove a container named 'nginx-web'
                sh 'docker stop nginx-web || true'
                sh 'docker rm nginx-web || true'
            }
        }
        stage('Deploy Container') {
            steps {
                // Run the newly built image, mapping host port (e.g., 8080) to container port 80
                sh "docker run -d --name nginx-web -p 9999:80 my-nginx-app:${env.BUILD_NUMBER}"
            }
        }
    }
    post {
        always {
            echo 'Pipeline finished.'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}
