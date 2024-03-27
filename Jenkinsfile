pipeline {
    agent any 
    environment {
        // Define environment variables
        JAVA_HOME = '/usr/bin/java'
        MAVEN_HOME = '/usr/bin/mvn'
        PATH = "${env.JAVA_HOME}/bin:${env.MAVEN_HOME}/bin:${env.PATH}"
    }
    tools {
        maven "maven3.6.3"
    
    }
    stages {
        
        stage('Install Dependencies') {
           steps {
               
               sh "mvn dependency:resolve"
           }
    }
        stage('Compile and Clean') { 
            steps {
              
                 sh "mvn clean"
        
        // Compile the source code
                 sh "mvn compile"
            }
        }
        stage('deploy') { 
            
            steps {
                sh "mvn package"
            }
        }
        stage('Build Docker image'){
          
            steps {
                echo "Hello Java"
                sh 'ls'
                sh "docker build -t moiz86/docker_jenkins_springboot:${BUILD_NUMBER} ."

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
               
                sh 'docker run -itd -p  8081:8080 new_container1/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}

