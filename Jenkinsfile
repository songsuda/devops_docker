pipeline {
    agent none
    environment {
        imageName = 'songsuda/my_web_ex'
        port = 80
    }
    
    stages {
       stage('Package') { 
          agent any
          steps {
              sh "docker --version"
//              sh "sudo usermod -aG docker $USER"
              sh "docker build -t ${imageName} ."
            withCredentials(
                [usernamePassword(credentialsId: 'docker_hub', 
                passwordVariable: 'dockerHubPassword', 
                usernameVariable: 'dockerHubUser')]) {
              sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
              sh "docker push ${imageName}"
            }
          }
       }

/*       stage('Deploy') { 
          agent {label 'mgr1'}
          steps {
           sh "echo h"
          }
       }
*/
       stage('Deploy') { 
          agent {label 'mgr1'}
          steps {
              script {
                  try {
                    sh "docker service update --image ${env.imageName} demo"
                    sh "echo update service"
                  } catch (e){
                    sh "docker service create --name demo -p 80:3000 ${env.imageName}"
                    sh "echo create service"
                  }
              }
          }
       }
       
    }
}