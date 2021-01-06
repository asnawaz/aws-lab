pipeline {
  agent any
  environment {
    PATH = "/opt/maven3/bin:$PATH"
    APP_NAME = 'awsdevops:latest'
    DOCKER_CRED = 'dockerhub_cred'
  }
  stages {
    stage("Git Checkout") {
      when {
        branch = 'dev'
      }
      steps {
        git credentialsId 'github', url: 'https:github.com/asnawaz/aws-lab'
      }
    }
    stage("Maven build") {
      when {
        branch = 'dev'
      }
      steps {
        sh "mvn clean package"
        sh "mv target/*.war target/myweb.war
      }
    }
   
    stage("Push") {
      when {
        branch = 'dev'
      }
      steps {
        script {
          docker.build("$APP_NAME")
          docker.withRegistry("", "DOCKER_CRED")
          docker.image("$APP_NAME).push()
        }
      }
    }
  }
