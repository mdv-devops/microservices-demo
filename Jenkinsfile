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
  
  }
}