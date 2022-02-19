pipeline {
    agent any 
    stages {
        stage('Compile and Clean') { 
            steps {

                sh "mvn clean compile"
            }
        }

        stage('Build JAR ') {
                    steps {

                        sh "mvn spring-boot:build-image"
                    }
                }
       

        stage('Build Docker image'){
            steps {
              
                sh 'docker build -t  mentalcoder1991/docker_jenkins_springboot:${BUILD_NUMBER} .'
            }
        }

        stage('Docker Login'){
            
            steps {
                 withCredentials([string(credentialsId: 'DockerId', variable: 'Dockerpwd')]) {
                    sh "docker login -u mentalcoder1991 -p ${Dockerpwd}"
                }
            }                
        }

        stage('Docker Push'){
            steps {
                sh 'docker push mentalcoder1991/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
        
        stage('Docker deploy'){
            steps {
               
                sh 'docker run -itd -p  9090:9090 mentalcoder1991/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }

        
        stage('Archiving') {
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}

