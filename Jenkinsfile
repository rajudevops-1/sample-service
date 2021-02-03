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
            "${scannerHome}/bin/sonar-scanner" -Dsonar.java.binaries=. -Dsonar.projectKey=citrinesample -Dsonar.host.url=http://54.242.133.133:9000 -Dsonar.sourceEncoding=UTF-8
        '''     
        } 
      
      }
    }

  stage('Docker build'){
   steps{
      sh '''
           docker build -t ${JOB_NAME}:${GIT_COMMIT} .
        '''    
      }
    }
  stage('Docker push to ECR'){
   steps{
      sh '''
           aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 236006218289.dkr.ecr.us-east-1.amazonaws.com
           docker tag ${JOB_NAME}:${GIT_COMMIT} 236006218289.dkr.ecr.us-east-1.amazonaws.com/${JOB_NAME}:${GIT_COMMIT}
           docker push 236006218289.dkr.ecr.us-east-1.amazonaws.com/${JOB_NAME}:${GIT_COMMIT}
        '''    
      }
    }
  
   stage('ws cleanup'){
      steps{
        cleanWs()
      }
    }
    stage(Deploy image){
        steps{
            sh '''
              docker pull 236006218289.dkr.ecr.us-east-1.amazonaws.com/${JOB_NAME}:${GIT_COMMT}
              docker run -d --name sample-app -p80:5000 --restart always 236006218289.dkr.ecr.us-east-1.amazonaws.com/${JOB_NAME}:${GIT_COMMT}
             '''
        }
    }
  }
}
