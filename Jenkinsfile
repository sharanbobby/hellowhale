pipeline {
    environment {
        registry = "sharanbobby/hellowhale"
        registryCredentials = 'b4f2e812-96a2-4285-b0b3-3e904d588e1f'
        dockerImage = ''
    }
    agent {
        kubernetes {
            defaultContainer 'jenkins-agent'
            inheritFrom 'kube'
            yaml '''
            spec:
              containers:
              - name: jenkins-agent
                image: sharanbobby/jenkinsbuild:1.3
                command:
                - cat
                tty: true
            '''
        
       }
    }
    
    
   stages {
       stage('Checkout') {
            steps {
                container('kubectl') {
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
                        kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "kubeconfig")
                   }

                
            }
        }
        
   }

}
