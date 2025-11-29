pipeline {
  agent any

  environment {
    IMAGE_NAME = 'devops-static-site'
    IMAGE_TAG = "v${BUILD_NUMBER}"
  }

  stages {

    stage('Checkout') {
      steps {
        echo '📥 Checking out code...'
        dir('/workspace') {
          sh 'ls -la'
        }
      }
    }

    stage('Build Docker Image') {
      steps {
        dir('/workspace/app') {
          echo '🐳 Building Docker image...'
          sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
          sh 'docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${IMAGE_NAME}:latest'
        }
      }
    }

    stage('Test Container') {
      steps {
        echo '🧪 Testing container...'
        sh 'docker run --rm ${IMAGE_NAME}:${IMAGE_TAG} nginx -t'
      }
    }

    stage('Deploy to Kubernetes') {
      steps {
        dir('/workspace') {
          echo '☸️ Deploying to Kubernetes...'
          
          script {
            // Update deployment with new image
            sh """
              kubectl set image deployment/devops-static-deployment \
                devops-static=${IMAGE_NAME}:${IMAGE_TAG} || \
              kubectl apply -f kubernetes/deployment.yaml
            """
            
            // Ensure service exists
            sh 'kubectl apply -f kubernetes/service.yaml'
            
            // Wait for rollout
            sh 'kubectl rollout status deployment/devops-static-deployment'
          }
        }
      }
    }

    stage('Verify Deployment') {
      steps {
        echo '✅ Verifying deployment...'
        sh 'kubectl get deployments'
        sh 'kubectl get pods'
        sh 'kubectl get services'
      }
    }

  }

  post {
    success {
      echo '🎉 Pipeline completed successfully!'
    }
    failure {
      echo '❌ Pipeline failed!'
    }
  }
}