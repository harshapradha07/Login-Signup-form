pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "harshapradha07/login-app"
        DOCKER_CREDENTIALS = "dockerhub-login"
    }

    stages {

        stage("Clone GitHub Repo") {
            steps {
                git url: "https://github.com/harshapradha07/Login-Signup-form.git",
                    branch: "main"
            }
        }

        stage("Build Docker Image") {
            steps {
                sh "docker build -t $DOCKER_IMAGE:latest ."
            }
        }

        stage("Push Image to DockerHub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "$DOCKER_CREDENTIALS",
                    usernameVariable: "DOCKER_USER",
                    passwordVariable: "DOCKER_PASS"
                )]) {
                    sh """
                    docker login -u $DOCKER_USER -p $DOCKER_PASS
                    docker push $DOCKER_IMAGE:latest
                    """
                }
            }
        }

        stage("Deploy to Kubernetes") {
            steps {
                sh """
                kubectl apply -f deployment.yaml
                kubectl rollout restart deployment login-deployment
                """
            }
        }
    }

    post {
        success {
            echo "✅ Login Signup Website Successfully Deployed!"
        }
    }
}
