pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-login')
        IMAGE_NAME = "taiebbsaies/devopstest"
    }

    stages {

        stage('Clone repository') {
            steps {
                git 'https://github.com/Smooky2/StudentsManagement-DevOps.git'
            }
        }

        stage('Build JAR with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                sh """
                    echo $DOCKERHUB_CREDENTIALS_PSW | docker login \
                        -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                """
            }
        }

        stage('Push Docker image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy container') {
            steps {
                sh 'docker stop devopstest || true'
                sh 'docker rm devopstest || true'
                sh 'docker run -d --name devopstest -p 8080:8080 $IMAGE_NAME:latest'
            }
        }
    }

    post {
        always {
            echo "Pipeline finished."
        }
    }
}
