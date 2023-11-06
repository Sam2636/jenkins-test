pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Test') {
            steps {
                sh 'pip install -r requirements.txt'
                sh 'python sample.py'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def imageName = "samdocker2636/sam_testscript:latest"
                    def dockerfile = "Dockerfile"  // Your Dockerfile path

                    docker.build(imageName, "-f ${dockerfile} .")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    def dockerHubCreds = credentials('docker01')
                    def imageName = "samdocker2636/sam_testscript:latest"
                    docker.withRegistry('https://index.docker.io/v1/', dockerHubCreds) {
                        docker.image(imageName).push()
                    }
                }
            }
        }
    }
}
