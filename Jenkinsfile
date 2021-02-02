pipeline{
  agent{label 'docker'}
  options{timeout (time: 1, unit:'HOURS')}
 stages{
    stage('SonarQube analysis'){
       environment {
        scannerHome = tool 'sonarscanner'
    }
      steps{
        withSonarQubeEnv('sonar'){
         sh ''' 
            "${scannerHome}/bin/sonar-scanner" 
        '''     
        } 
         timeout(time: 1, unit: 'HOURS') {
        waitForQualityGate abortPipeline: true
        }
      }
    }

  stage('Docker build'){
   steps{
      sh '''
           docker build -t ${GIT_COMMIT} .
        '''    
      }
    }
   stage('ws cleanup'){
      steps{
        cleanWs()
      }
    }
  }
}
