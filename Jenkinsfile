pipeline {  
    agent any  
        stages {
            stage('Checkout') {
                steps {
                    checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Krishna238-hub/onlinebookstore-master.git']])
                    
                }
                
            }
            stage("build maven") {  
           	    steps {  
                    sh "mvn clean install"
              	    }
                
            }
            stage("build docker Image") {  
           	    steps {  
                      sh "docker build -t bookstore:1 ."
              	    }
                
            }
            stage("Build Tag"){
                steps{
                   sh "docker tag bookstore:1  krishna2317/onlinebookstore:2" 
                }
            }
            stage("Push to docker Hub") {
                steps {
                    script{
                        withDockerRegistry(credentialsId: 'docker-credentials', url: 'https://index.docker.io/v1/') {
                            sh "echo DOCKER_PAT | docker login -u krishna2317 --password-stdin https://index.docker.io/v1/"
                            sh "docker push krishna2317/onlinebookstore:2"
                            
                        }
                    }
                }
            }  
        }
}
