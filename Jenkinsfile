pipeline {
    environment {
        registry = "sharanbobby/hellowhale"
        registryCredentials = 'b8e75826-b643-4980-8fa7-0283b1072574'
        dockerImage = ''
    }
    agent any

   stages {
        stage('Checkout') {
            steps {

               git url:'https://github.com/sharanbobby/hellowhale', branch:'master'
                      
            }
        }
   }
        stage('Build') {
            steps {

                    script {
                            dockerImage = docker.build registry + ":$Build_Number" 
                    }   
                 
            }
        }
        stage('Push image to docker') {
            steps {

                    script {
                        docker.withRegistry( '', registryCredentials ) {
                            dockerImage.push("latest")

                        }
                    }
                
            }
        }
        stage('Deploy') {
            steps {

                    script {
                        kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "dockerkubeconfig")
                   }

                
            }
        }
 }
