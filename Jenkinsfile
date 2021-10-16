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

    stage('Kaniko Build & Push Image') {
      steps {
        container('kaniko') {
          script {
            sh '''
            /kaniko/executor --dockerfile `pwd`/src/adservice/Dockerfile \
                             --context `pwd` \
                             --destination=marinyuk/adservice:${BUILD_NUMBER}
            '''
            sh '''
            /kaniko/executor --dockerfile `pwd`/src/cartservice/Dockerfile \
                             --context `pwd` \
                             --destination=marinyuk/cartservice:${BUILD_NUMBER}
            '''
            sh '''
            /kaniko/executor --dockerfile `pwd`/src/checkoutservice/Dockerfile \
                             --context `pwd` \
                             --destination=marinyuk/checkoutservice:${BUILD_NUMBER}
            '''
            sh '''
            /kaniko/executor --dockerfile `pwd`/src/currencyservice/Dockerfile \
                             --context `pwd` \
                             --destination=marinyuk/currencyservice:${BUILD_NUMBER}
            '''
            sh '''
            /kaniko/executor --dockerfile `pwd`/src/emailservice/Dockerfile \
                             --context `pwd` \
                             --destination=marinyuk/emailservice:${BUILD_NUMBER}
            '''
            sh '''
            /kaniko/executor --dockerfile `pwd`/src/frontend/Dockerfile \
                             --context `pwd` \
                             --destination=marinyuk/frontend:${BUILD_NUMBER}
            '''
            sh '''
            /kaniko/executor --dockerfile `pwd`/src/loadgenerator/Dockerfile \
                             --context `pwd` \
                             --destination=marinyuk/loadgenerator:${BUILD_NUMBER}
            '''
            sh '''
            /kaniko/executor --dockerfile `pwd`/src/paymentservice/Dockerfile \
                             --context `pwd` \
                             --destination=marinyuk/paymentservice:${BUILD_NUMBER}
            '''
            sh '''
            /kaniko/executor --dockerfile `pwd`/src/productcatalogservice/Dockerfile \
                             --context `pwd` \
                             --destination=marinyuk/productcatalogservice:${BUILD_NUMBER}
            '''
            sh '''
            /kaniko/executor --dockerfile `pwd`/src/recommendationservice/Dockerfile \
                             --context `pwd` \
                             --destination=marinyuk/recommendationservice:${BUILD_NUMBER}
            '''
            sh '''
            /kaniko/executor --dockerfile `pwd`/src/shippingservice/Dockerfile \
                             --context `pwd` \
                             --destination=marinyuk/shippingservice:${BUILD_NUMBER}
            '''
          }
        }
      }
    }

    stage('Deploy App to Kubernetes') {     
      steps {
        container('kubectl') {
          withCredentials([file(credentialsId: 'mykubeconfig', variable: 'KUBECONFIG')]) {
            sh 'sed -i "s/<TAG>/${BUILD_NUMBER}/" myweb.yaml'
            sh 'kubectl apply -f myweb.yaml'
          }
        }
      }
    }
  
  }
}