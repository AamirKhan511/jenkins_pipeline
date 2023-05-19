def application = "devops"
pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
       stage('clone') {
      steps {
        git 'https://github.com/AamirKhan511/jenkins_pipeline.git'
      }
    }
    stage('Build') {
      steps {
        sh 'sudo docker build -t aamir335/app:${BUILD_NUMBER} .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'sudo docker push aamir335/app:${BUILD_NUMBER}'
      }
    }
   stage('Remove running container with old code'){
          steps {
              sh 'docker rm -f \$(docker ps -a -f name=devops -q) || true' 
            }
         }
   stage('Docker deploy'){
            steps {
                sh 'docker run --name devops -d -p 80:80 aamir335/app:${BUILD_NUMBER}'
            }
        }
    stage('Remove old images') {
          steps{
               sh("docker rmi aamir335/app:latest -f")
           }
       } 
   
    
  }
  post {
    always {
      sh 'sudo docker logout'
    }
  }
}

