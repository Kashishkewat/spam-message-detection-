pipeline {
  agent any

  environment {
    IMAGE_NAME = "spam-app"
    CONTAINER_NAME = "spam-container"
    PORT = "8501"
  }

  stages {

    stage('Clone Code') {
      steps {
        git 'https://github.com/Kashishkewat/spam-message-detection-.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
        docker build -t $IMAGE_NAME .
        '''
      }
    }

    stage('Stop & Remove Old Containers') {
      steps {
        sh '''
        docker stop spam-container || true
        docker rm spam-container || true

        # remove any old random containers using spam-app
        docker ps -a | grep spam-app | awk '{print $1}' | xargs -r docker rm -f
        '''
      }
    }

    stage('Run New Container') {
      steps {
        sh '''
        docker run -d -p 8501:8501 \
        --name $CONTAINER_NAME \
        $IMAGE_NAME
        '''
      }
    }

    stage('Cleanup Old Images') {
      steps {
        sh 'docker image prune -f'
      }
    }
  }

  post {
    success {
      echo '✅ App deployed successfully on port 8501'
    }
    failure {
      echo '❌ Deployment failed'
    }
  }
}
