pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "harshapradha07/login-app"
    }

    stages {

        stage("Build Docker Image") {
            steps {
                bat "docker build -t %DOCKER_IMAGE%:latest ."
            }
        }

        stage("Push Image to DockerHub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerhub-login",
                    usernameVariable: "DOCKER_USER",
                    passwordVariable: "DOCKER_PASS"
                )]) {
                    bat """
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    docker push %DOCKER_IMAGE%:latest
                    """
                }
            }
        }

        stage("Deploy to Kubernetes") {
            steps {
                bat """
                kubectl apply -f deployment.yaml
                kubectl rollout restart deployment login-deployment
                """
            }
        }
    }
}
