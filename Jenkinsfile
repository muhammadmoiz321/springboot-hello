pipeline {
    agent any 
    tools {
        maven "maven3.6.3"
    
    }
    stages {
        stage('Compile and Clean') { 
            steps {
              
                sh "mvn clean compile"
            }
        }
        stage('deploy') { 
            
            steps {
                sh "mvn package"
            }
        }
        stage('Build Docker image'){
          
            steps {
                echo "Hello Java Express"
                sh 'ls'
                sh 'docker build -t  moiz86/docker_jenkins_springboot:${BUILD_NUMBER} .'
            }
        }
        stage('Docker Login'){
            
            steps {
                 withCredentials([string(credentialsId: 'moiz86', variable: 'Dockerpwd')]) {
                    sh "docker login -u moiz86 -p ${Dockerpwd}"
                }
            }                
        }
        stage('Docker deploy'){
            steps {
               
                sh 'docker run -itd -p  8081:8080 new_container/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}

