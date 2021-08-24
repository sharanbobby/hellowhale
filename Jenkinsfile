pipeline {
    environment {
        registry = "sharanbobby/hellowhale"
        registryCredentials = 'b4f2e812-96a2-4285-b0b3-3e904d588e1f'
        dockerImage = ''
    }
    agent {
        kubernetes {
            yaml '''
            spec:
              containers:
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
                container('kubectl') {
                        git url:'https://github.com/sharanbobby/hellowhale', branch:'master'
                       
                }                      
            }
        }
   }
}
