pipeline {
    agent any
    environment
    {
        VERSION = "${BUILD_NUMBER}"
        IMAGE = 'nodeapp'
      
        ECRCRED = 'ecr:us-east-1:registrycred'
        ECRURL = 'https://279214042703.dkr.ecr.us-east-1.amazonaws.com/nodeapp'
        
    }
    
    stages {
        stage('GitCheckOut') {
            steps {
                git credentialsId: 'githubcred', url: 'https://github.com/devopsninja464/web_login_automation.git'
            }
        }
         stage('DockerBuild') {
            steps {
                script {
                    dockerImage = docker.build  IMAGE + ":$BUILD_NUMBER"
                }
            }
        }
        
         stage('DockerPushToECR') {
            steps {
                script {
                     // withDockerRegistry(credentialsId: 'ecr:us-east-1:registrycred', url: '279214042703.dkr.ecr.us-east-1.amazonaws.com/nodeapp') { 
                    docker.withRegistry(ECRURL, ECRCRED) {
                       dockerImage.push()
                   }
                   }
                }
            }
            
            stage('Deploy') {
            steps {
                script {
                     sh '''#!/bin/bash
                     python3 -m venv venv
                     chmod +x ./venv/bin/activate
                     source ./venv/bin/activate
                     pip3 install --upgrade pip
                     pip3 install -r requirements.txt
                     ansible-playbook deploy_node_webapp.yml -i devinventory
         '''
                 
                   }
                   }
                }
            }
        
}