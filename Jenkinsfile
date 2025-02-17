pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'jpetstore-image'
        DOCKER_REPO = 'myousry009/jpetstore-image'
        DOCKER_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/MohamedYousry17/simple-Java-web-app.git'
            }
        }

        stage('Build & Test') {
            steps {
                script {

                    sh 'mvn clean package'

                    sh 'mvn test'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {

                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {

                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        
                     

                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'


                        sh 'docker tag $DOCKER_IMAGE $DOCKER_REPO:$DOCKER_TAG'
                        sh 'docker push $DOCKER_REPO:$DOCKER_TAG'
                    }
                }
            }
        }

        stage('Run Container with Ansible') {
            steps {
                script {

                 sh 'ansible-playbook -i inventory.ini run_container.yml'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline executed successfully!'
        }
        failure {
            echo '❌ Pipeline execution failed!'
        }
    }
}

