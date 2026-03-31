pipeline {
  agent any

  environment {
    IMAGE_NAME = "spam-classifier"
    CONTAINER_NAME = "spam-container"
  }

  stages {

    stage('Clone Code') {
      steps {
        git 'https://github.com/your-repo.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t $IMAGE_NAME .'
      }
    }

    stage('Stop Old Container') {
      steps {
        sh 'docker stop $CONTAINER_NAME || true'
        sh 'docker rm $CONTAINER_NAME || true'
      }
    }

    stage('Run Container') {
      steps {
        sh 'docker run -d -p 80:8501 --name $CONTAINER_NAME $IMAGE_NAME'
      }
    }

  }
}
