pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'test3'
        CONTAINER_NAME = 'lala'
        PORT_MAPPING = '8082:80'  // Adjust the port mapping as needed
    }

    stages {
        // stage('Checkout') {
        //     steps {
        //         // Clean workspace before checkout
        //         deleteDir()
        //         // Checkout the HTML source code from GitHub
        //         git url: 'https://github.com/andrinahaura/project1.git'
        //     }
        // }
            stage('Checkout') {
                steps {
                    deleteDir()
                    checkout([$class: 'GitSCM', branches: [[name: 'master']], userRemoteConfigs: [[url: 'https://github.com/atoschova/project_to_the_bone.git/']]])
                          // Tambahkan pernyataan log untuk menampilkan direktori saat ini
                }
            }

        stage('Build Docker Image') {
            steps {
                script {
                      sh 'ls -l'
                    dir('testdeploy') {
                        // Build Docker image dengan konten HTML
                        // sh 'docker build -t test3 -f Dockerfile .'
                        docker.build("${DOCKER_IMAGE}",'-f Dockerfile .')
                    }
                    // // Build Docker image with the HTML content
                    // docker.build("${DOCKER_IMAGE}", '-f Dockerfile .')
                }
            }
        }

    
        stage('Run Docker Container') {
            steps {
                script {
                    // Run Docker container based on the built image
                    docker.image("${DOCKER_IMAGE}").run("-p ${PORT_MAPPING} --name ${CONTAINER_NAME}")
                }
            }
        }

        stage('Run Docker') {
            steps {
                script {
                    // Run Docker container based on the built image
                    docker.image('some-content-nginx').inside {
                        sh 'docker run --name some-nginx -d -p 8081:80 some-content-nginx'
                    }
                }
            }
        }

        stage('Run Docker') {
            steps {
                script {
                    // Run Docker container based on the built image
                    dir('project_to_the_bone')
                    docker.image('some-content-nginx').inside {
                        sh 'docker run --name some-nginx -d -p 8081:80 some-content-nginx'
                    }
                }
            }
        }




    }

    post {
        always {
            script {
                // Stop and remove the Docker container after execution
                docker.image("${DOCKER_IMAGE}").stop()
                docker.image("${DOCKER_IMAGE}").remove()
            }
        }
    }
}