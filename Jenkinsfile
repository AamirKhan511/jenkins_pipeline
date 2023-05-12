pipeline{

    agent any

    environment {
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
    }

    stages {
        
        stage('gitclone') {

            steps {
                git 'https://github.com/AamirKhan511/jenkins_pipeline.git'
            }
        }

        stage('Build') {

            steps {
                sh '/usr/bin/docker build -t aamir335/app:latest .'
            }
        }

        stage('Login') {

            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push') {

            steps {
                sh 'docker push aamir335/app:latest'
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
    }

}