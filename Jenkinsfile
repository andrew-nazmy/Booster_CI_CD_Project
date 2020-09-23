pipeline {
    agent {label 'slave'}

    stages {
        stage ('preparation'){
        steps{
            git'https://github.com/andrew-nazmy/Booster_CI_CD_Project.git'
            }
         }   
        stage('Build') {
                steps {
                    sh 'docker build -f Dockerfile . -t nazmy123/django_app:v1.0'
                      }
                   
            }
        stage('Push') {
            steps {
                     withCredentials([usernamePassword(credentialsId:"docker",usernameVariable:"USERNAME",passwordVariable:"PASSWORD")]){
                sh 'docker login --username $USERNAME --password $PASSWORD'
                sh 'docker push nazmy123/django_app:v1.0'
                }
            }
        }
         stage('Deploy') {
            steps {
                sh 'docker run -d -p 3000:3000 nazmy123/django_app:v1.0'
            }  
    }
  }
post {
    success{
     slackSend(color: '#00FF00',message: "DONE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'(${env.BUILD_URL}console)")
           }
    failure {
      slackSend(color: '#FF0000',message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'(${env.BUILD_URL}console)")
                      }
}
}
