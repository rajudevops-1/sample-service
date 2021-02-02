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
      
      }
    }

  stage('Docker build'){
   steps{
      sh '''
           docker build -t sample-service:${GIT_COMMIT} .
        '''    
      }
    }
  stage('Docker build'){
   steps{
      sh '''
           aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 236006218289.dkr.ecr.us-east-1.amazonaws.com
           docker tag sample-service:${GIT_COMMIT} 236006218289.dkr.ecr.us-east-1.amazonaws.com/sample-service:${GIT_COMMIT}
           docker push 236006218289.dkr.ecr.us-east-1.amazonaws.com/sample-service:${GIT_COMMIT}
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
