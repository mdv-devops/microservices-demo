pipeline {

  options {
    ansiColor('xterm')
  }

  agent {
    kubernetes {
      yamlFile 'builder.yaml'
    }
  }

  stages {

    stage('Kaniko Build & Push "adservice" Image') {
      steps {
        container('kaniko') {
          script {
            sh '''
            /kaniko/executor --dockerfile `pwd`/src/adservice/Dockerfile \
                             --context `pwd`/src/adservice/ \
                             --destination=marinyuk/adservice:${BUILD_NUMBER}
            '''
          }
        }
      }
    }
    
    stage('Deploy App to Kubernetes') {     
      steps {
        container('kubectl') {
          withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
            sh 'sed -i "s/<TAG>/${BUILD_NUMBER}/" kubernetes-manifests/adservice.yaml'
            sh 'kubectl apply -f kubernetes-manifests/adservice.yaml'
          }
        }
      }
    }

    stage('Kaniko Build & Push "cartservice" Image') {
      steps {
        container('kaniko') {
          script {
            sh '''
            /kaniko/executor --dockerfile `pwd`/src/cartservice/src/Dockerfile \
                             --context `pwd`/src/cartservice/src/ \
                             --destination=marinyuk/cartservice:${BUILD_NUMBER}
            '''
          }
        }
      }
    }

    stage('Deploy App to Kubernetes') {     
      steps {
        container('kubectl') {
          withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
            sh 'sed -i "s/<TAG>/${BUILD_NUMBER}/" kubernetes-manifests/cartservice.yaml'
            sh 'kubectl apply -f kubernetes-manifests/cartservice.yaml'
          }
        }
      }
    }
  
  }
}