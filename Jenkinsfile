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
                      sh "docker build -t bookstore:${BUILD_NUMBER} ."
              	    }
                
            }
            stage("Build Tag"){
                steps{
                   sh "docker tag bookstore:${BUILD_NUMBER}  krishna2317/onlinebookstore:${BUILD_NUMBER}" 
                }
            }
            stage("Push to docker Hub") {
                steps {
                    script{
                       withCredentials([string(credentialsId: 'docker-pat', variable: 'Docker_PAT')]) {
                           sh 'echo $Docker_PAT | docker login -u krishna2317 --password-stdin https://index.docker.io/v1/'
                           sh "docker push krishna2317/onlinebookstore:${BUILD_NUMBER}"
                            
                        }
                    }
                }
            }  
            stage("deploy to Tomact"){
                steps{
                    deploy adapters: [tomcat9(credentialsId: 'admin', path: '', url: 'http://65.1.127.232:8090')], contextPath: '/docker', war: '**/*.war'
                }
            }
            stage("pull to Docker desktop"){
                steps{
                    sh 'docker pull krishna2317/onlinebookstore:${BUILD_NUMBER}'
                }
            }
            stage("run"){
                steps{
                    sh 'docker run -d -p 8040:8080 bookstore:${BUILD_NUMBER}'
                }
                
            }
            
        }
}
