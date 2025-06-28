pipeline{
    agent any
    environment{
        IMAGE_NAME = "22monk/docker-repo"
        IMAGE_TAG = "${IMAGE_NAME}:${env.BUILD_NUMBER}"
    }
    stages{
        stage('Clone Repo'){
            steps{
                sh 'rm -rf repo-folder'
                sh 'git clone https://github.com/chris-kyle2/github-repo.git'
            }
        }
        stage('Setup Environment'){
            steps{
                pip install -r repo-folder/requirements.txt
            }
        }
        stage('Unit Testing'){
            steps{
                sh 'pytest'
            }
        }
        stage('Build Docker Image'){
            steps{
                sh "docker build -t ${IMAGE_TAG} ."
            }
        }
        stage('Push Docker Image'){
            steps{
                sh "docker push ${IMAGE_TAG}"
            }
        }
        stage('Approval Required'){
            steps{
                script{
                    def userInput = input(
                        id: 'userinput',
                        message: 'Approve and provide Docker Image',
                        parameters: [
                            string(
                                defaultValue: 'defaultvalue',
                                description: 'Docker Image'
                            )
                        ]
                    )
                    env.DOCKER_IMAGE = userInput
                }
            }
        }
        stage('Deploy Docker Image'){
            steps{
                echo "Deploying Docker Image: ${env.DOCKER_IMAGE}"
                sh "docker run -d ${env.DOCKER_IMAGE}"
            }
        }
    }
}