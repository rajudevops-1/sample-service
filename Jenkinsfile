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
            "${scannerHome}/bin/sonar-scanner" -Dsonar.java.binaries=. -Dsonar.projectKey=citrinesample -Dsonar.host.url=http://35.168.13.119:9000 -Dsonar.sourceEncoding=UTF-8
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
