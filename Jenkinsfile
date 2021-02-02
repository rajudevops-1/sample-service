pipeline{
  agent{label 'docker'}
  options{timeout (time: 1, unit:'HOURS')}
 stages{
    stage('SonarQube analysis'){
      steps{
        withSonarQubeEnv ('sonar'){
         sh ''' 
            sonar-scanner -Dsonar.java.binaries=. -Dsonar.projectKey=citrinesample
        '''     
        } 
      }
    }
  stage('Quality Gate'){
      steps{
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
