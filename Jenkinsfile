pipeline {  
  agent any
  environment {
    PROJECT='firstPipeline'
  }

  stages {
    stage('dev') {
      steps {
        sh '''
          echo This is "$PROJECT on DEV environment"
        '''
      }
    }
  }

}