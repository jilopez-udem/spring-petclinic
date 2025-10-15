#!groovy

pipeline {
  agent none
  stages {
    stage('Maven Install') {
      agent {
        docker {
          image 'maven:3.9-eclipse-temurin-17' 
          args '-v $HOME/.m2:/root/.m2'      
          reuseNode true
        }
      }
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t jilopezv/spring-petclinic:latest .'
      }
    }
  }
}
