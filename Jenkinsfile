def strangler_images = ["legacy-edge","customer-service","edge-service","profile-web","discovery-service","config-service","user-service","profile-service"]

pipeline {
    agent {label 'node2'}
    

    environment {
        legacyedge =''
        customerservice = ''
        registryCredential = 'fasdamc-dockerhub'

    }
    
    stages {
        stage('Checkout Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'Github-credential', url: 'https://github.com/fasdamc/cloud-native-microservice-strangler-example.git']]])
            }
        }
        stage('Build') {
            steps {
                script {
				    sh "mvn clean install"
                }
            }
        }
        stage ('Tag Docker Image') {
            steps {
                script {
				    strangler_images.each { name->
				        sh "docker tag ${name}:latest fasdamc/${name}"
                }
            }
        }

        }
        
        stage ('Upload Image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        strangler_images.each { name->
                            sh "docker push fasdamc/${name}"
                        }
                    }
                }
            }
        }
  
    }
}
