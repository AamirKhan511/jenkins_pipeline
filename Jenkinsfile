def application = "devops"
def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
]
def getBuildUser() {
    return currentBuild.rawBuild.getCause(Cause.UserIdCause).getUserId()
}
pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
      test variable: 0=success, 1=fail; //must be string
        doError = '0'
        BUILD_USER = ''
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
    stage('Error') {
            // when doError is equal to 1, return an error
            when {
                expression { doError == '1' }
            }
            steps {
                echo "Failure :("
                error "Test failed on purpose, doError == str(1)"
            }
        } 
    stage('Success') {
            // when doError is equal to 0, just print a simple message
            when {
                expression { doError == '0' }
            }
            steps {
                echo "Success :)"
            }
        }    
  }
  post {
    always {
      sh 'sudo docker logout'
    }
  }
}
   post {
        always {
            script {
                BUILD_USER = getBuildUser()
            }
            echo 'I will always say hello in the console.'
            slackSend channel: '#slack-test-channel',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} by ${BUILD_USER}\n More info at: ${env.BUILD_URL}"
        }
    }
