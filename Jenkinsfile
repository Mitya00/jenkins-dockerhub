
pipeline{

 agent any
 environment {
    DOCKERHUB_CREDENTIALS=credentials('dockerhub')
 }
 triggers {
  upstream 'day-2, '
 }

 stages {

  stage('Build') {

   steps {
    sh 'sudo docker build -t mintemir/awesome-cat-front awesome_cats_frontend'
   }
  }
  stage('Build2') {

   steps {
    sh 'sudo docker build -t mintemir/awesome-cat-back awesome_cats_backend'
   }
  }
  stage('Login') {

    steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
    }
  }
  stage('Push') {

    steps {
        sh 'sudo docker push mintemir/awesome-cat-front'
    }
  }
  stage('Push2') {

    steps {
        sh 'sudo docker push mintemir/awesome-cat-back'
    }
  }
 }
 post {
    always {
        sh 'docker logout'
    }
    failure {
        emailext to: "kurbanaliev.mintemir@gmail.com",
        subject: "jenkins build:${currentBuild.currentResult}: ${env.JOB_NAME}",
        body: "${currentBuild.currentResult}: Job ${env.JOB_NAME}\nMore Info can be found here: ${env.BUILD_URL}"
    }
 } 
}
