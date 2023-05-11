node{
    stage('Scm checkout'){
       git 'https://github.com/AamirKhan511/jenkins_pipeline.git'
    }
    
     stage('Bulid Docker Image'){
    sh 'docker build -t aamir335/website .'
    }
}