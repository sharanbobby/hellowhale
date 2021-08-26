pipeline {
    environment {
        registry = "sharanbobby/hellowhale"
        registryCredentials = '3c5d7f13-f263-4109-9d40-ab64bff290b1'
        dockerImage = ''
    }
    agent any

   stages {
        
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

}
