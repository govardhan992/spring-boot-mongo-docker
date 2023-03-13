pipeline {
    agent any
     tools {
          maven 'maven 3.9.0' 
    }
    stages {
        stage('check out'){
            steps {
                git 'https://github.com/govardhan992/java-web-app-docker.git'
                sh 'ls -l'
            }
        }
        stage('build'){
            steps {
                sh "mvn clean package"
            }
        }
         stage('build the docker image'){
            steps {
                sh "docker build -t govardhanr992/spring-boot-mongo:${BUILD_NUMBER} ."
            }
        }
        stage("push docker image"){
            steps {
               withCredentials([string(credentialsId: 'docker_hub_pwd', variable: 'docker_hub_pwd')]) {
                
               sh "docker login -u govardhanr992 -p ${docker_hub_pwd}"
     }
                sh "docker push govardhanr992/spring-boot-mongo:${BUILD_NUMBER}"
            }
        }
         // Remove local image in Jenkins Server
    stage("Remove Local Image"){
        steps {
        sh "docker rmi -f dockerhandson/spring-boot-mongo"
        }
        }
        stage('docker swarm setup'){
            steps {
                script {
                    sshagent(['docker_swarm']) {
                    sh 'scp -o StrictHostKeyChecking=no  docker-compose.yml ubuntu@172.31.11.106:' 
                    sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.11.106 docker stack deploy --prune --compose-file docker-compose.yml springboot'
                  }
                }
            }
        }
     }
    }
