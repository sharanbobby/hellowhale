pipeline {
    environment {
        registry = "sharanbobby/hellowhale"
        registryCredentials = 'b4f2e812-96a2-4285-b0b3-3e904d588e1f'
        dockerImage = ''
    }
   agent any
   stages {
        stage('Checkout') {
            steps {
                git url:'https://github.com/sharanbobby/hellowhale', branch:'master'
            }
        }
        stage('Build') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$Build_Number"
                }
            }
        }
        stage('Push image to docker') {
            steps {
                scripts {
                    docker.withRegistry( '', registryCredentials) {
                        dockerImage.push("latest")
                        
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    kubernetesDeploy(configs: "hellowhale.yml", kubeconfigID: "dockerkubeconfig")
                }
            }
        }
    }
 }
