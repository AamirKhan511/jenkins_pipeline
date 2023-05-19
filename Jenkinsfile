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
   stage('Docker deploy'){
            steps {
               
                sh 'docker run -itd -p  80:80 aamir335/app:${BUILD_NUMBER}'
            }
        }
  }
  post {
    always {
      sh 'sudo docker logout'
    }
  }
}