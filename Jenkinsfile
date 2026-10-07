#!groovy

// Kubernetes/Kaniko version of the spring-petclinic Jenkinsfile.
// Copy it over the Jenkinsfile in your spring-petclinic repo.
pipeline {
  agent {
    kubernetes {
      yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: maven
    image: maven:3.9-eclipse-temurin-25
    command: ["cat"]
    tty: true
  - name: kaniko
    image: gcr.io/kaniko-project/executor:debug
    command: ["/busybox/cat"]
    tty: true
    volumeMounts:
    - name: docker-config
      mountPath: /kaniko/.docker
  volumes:
  - name: docker-config
    secret:
      secretName: regcred
      items:
      - key: .dockerconfigjson
        path: config.json
'''
    }
  }
  environment {
    IMAGE = 'docker.io/kevinmateog/spring-petclinic:gestion-udem-jenkins'
  }
  stages {
    stage('Maven Install') {
      steps {
        container('maven') {
          sh 'mvn -B clean package -DskipTests'
        }
      }
    }
    stage('Docker Build & Push') {
      steps {
        container('kaniko') {
          sh '/kaniko/executor --context "$WORKSPACE" --dockerfile "$WORKSPACE/Dockerfile" --destination "$IMAGE"'
        }
      }
    }
  }
}
