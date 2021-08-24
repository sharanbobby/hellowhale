pipeline {
    environment {
        registry = "sharanbobby/hellowhale"
        registryCredentials = 'b4f2e812-96a2-4285-b0b3-3e904d588e1f'
        dockerImage = ''
    }
    agent {
        kubernetes {
            defaultContainer 'jenkins-slave'
            inheritFrom 'kube'
            yaml '''
            spec:
              containers:
              - name: git
                image: bitnami/git:latest
                command:
                - cat
                tty: true
              - name: kubectl
                image: bitnami/kubectl:latest
                command:
                - cat
                tty: true
              - name: docker
                image: docker:latest
                command:
                - cat
                tty: true 
           '''
        }
   }
   stages {
        stage('Checkout') {
            steps {
                container('git') {
                    dir('/home/jenkins/agent/workspace') {
                        git url:'https://github.com/sharanbobby/hellowhale', branch:'master'
                    }    
                }                      
            }
        }

        stage('Build') {
            steps {
                container('docker') {
                    script {
                        dir('/home/jenkins/agent/workspace') {
                            dockerImage = docker.build registry + ":$Build_Number"
                        }  
                    }   
                }  
            }
        }
        stage('Push image to docker') {
            steps {
                container('docker') {
                    script {
                        docker.withRegistry( '', registryCredentials ) {
                            dockerImage.push("latest")

                        }
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                container('kubectl') {
                    script {
                        kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "dockerkubeconfig")
                   }

                }
            }
        }
    }
 }
