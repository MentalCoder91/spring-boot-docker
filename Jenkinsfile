pipeline {
    agent any 
    stages {
        stage('Compile and Clean') { 
            steps {

                sh "mvn clean compile"
            }
        }

        stage('Clean Install') {
                    steps {

                        sh "mvn clean install"
                    }
                }


        stage('deploy') {
            steps {
                sh "mvn package"
            }
        }


        stage('Build Docker image'){
            steps {
              
                sh 'docker build -t  mentalcoder1991/docker_jenkins_springboot:${BUILD_NUMBER} .'
            }
        }

        stage('Docker Login'){
            
            steps {
                 withCredentials([string(credentialsId: 'DockerId', variable: 'DockerPassword')]) {
                    sh "docker login -u mentalcoder1991 -p ${DockerPassword}"
                }
            }                
        }

        stage('Docker Push'){
            steps {
                sh 'docker push mentalcoder1991/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }
       
        stage('Docker Stop'){
            steps {
               
                sh 'docker stop dockerspringboot '
            }
        }
        
        stage('Docker deploy'){
            steps {
               
                sh 'docker run -itd --name dockerspringboot -p  9090:9090 mentalcoder1991/docker_jenkins_springboot:${BUILD_NUMBER}'
            }
        }

        
        stage('Archiving') {
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}

