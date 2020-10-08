pipeline {  
  agent any
  environment {
    PROJECT='firstPipeline'
  }

  stages {
    stage('Build') {
      environment {
        LOG_LEVEL='INFO'
      }      
      parallel {
        stage("linux"){
          steps {
            echo "Building ${PROJECT} with log level ${LOG_LEVEL} on linux"
            sh 'chmod +x scripts/build.sh'
            withCredentials([string(credentialsId: 'my-key-here', variable: 'API_KEY')]){
              sh '''
                ./scripts/build.sh
              '''
            }                   
          }
        }
        stage("windows"){
          steps {
            echo "Building ${PROJECT} with log level ${LOG_LEVEL} on windows"        
          }
        }
      }
    }
    stage('Test') {            
      steps {
        echo "Testing ${PROJECT}"       
        writeFile file: "test-results.txt", text: "passed" 
      }
    }
    stage('Deploy') {
      input {
        message 'Deploy?'
        ok 'Do it'
        parameters {
          string(name: 'TARGET_ENVIRONMENT', defaultValue: 'PROD', description: 'Target deployment environment')
        }
      }      
      steps {
        echo "Deploting ${PROJECT} to ${TARGET_ENVIRONMENT} environment"        
      }
    }
  }
  post{
    always {
      echo 'Always print this'
    }
    success {
      archiveArtifacts "test-results.txt"
    }
  }
}