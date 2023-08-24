pipeline {
    agent {label 'dev'}
     stages{
         stage('code'){
             steps{
                 script{
                     properties([pipelineTriggers([pollSCM('')])])
                 }
                 git url: 'https://github.com/sunil1397/node-todo-cicd.git', branch: 'master'
             }
         }
         stage('Build and testing'){
             steps{
                 sh 'docker build . -t sunil1397/node-todo-app-cicd:latest'
             }
         }
         stage('Login and push image'){
             steps{
                 echo 'login and push image'
                 withCredentials([usernamePassword(credentialsId:'dockerHub', passwordVariable:'dockerHubPassword', usernameVariable:'dockerHubUser')]){
                     sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                     sh "docker push sunil1397/node-todo-app-cicd:latest"
                 }
             }
         }
         stage('Deployed'){
             steps{
                 sh 'docker-compose down && docker-compose up -d'
             }
         }
     }
}
