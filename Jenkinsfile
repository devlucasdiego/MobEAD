pipeline {
    agent any

    environment {
        registry = "lucasdiego/mobead_image_build"
        registryCredential = 'Dockerhub'
        dockerImage = 'image=$image'
    }

    stages {
        stage('Checkout') {
            steps {
                // Fazendo o checkout do repositório do GitHub
                git branch: 'feature-ci-cd', url: 'https://github.com/devlucasdiego/MobEAD.git'
            }
        }

        stage('Build image') {
            steps {
                script {
                    // Verifica se o Dockerfile está presente
                    sh 'ls -lah'  // Isso ajuda a verificar se o Dockerfile está no diretório correto
                    // Construa a imagem Docker com base no Dockerfile presente
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }

        stage('Delivery image') {
            steps {
                script {
                    // Fazendo o push da imagem para o Docker Hub
                    docker.withRegistry('https://registry-1.docker.io/v2/', 'Dockerhub') {
                        dockerImage.push("$BUILD_NUMBER")
                    }
                }
            }
        }
    }
}

