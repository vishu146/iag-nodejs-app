pipeline {
 agent any
 options {
  skipDefaultCheckout()
 }
 stages {
  stage('SCM') {
   steps {
    checkout scm
   }
  }
  stage('Build') {
    agent {
     docker {
      image 'node:latest'
      reuseNode true
      }
     }
     steps {
      sh 'npm install'
     }
   }
  stage('Unit Tests') {
   agent {
    docker {
     image 'node:latest'
     reuseNode true
    }
   }
   steps {
    sh 'node app.js &'
    sh 'npm test'
   } 
  }
  stage('Static Code Analysis') {
   agent {
    docker {
     image 'node:latest'
     reuseNode true
    }
   }
   steps {
    sh 'echo linting check...'
   } 
  }  
  stage('Code Coverage') {
   agent {
    docker {
     image 'node:latest'
     reuseNode true
    }
   }
   steps {
    sh 'node app.js &'    
    sh 'npm run coverage'
    archiveArtifacts artifacts: '**/coverage/*.html', fingerprint: false
   } 
  }
  stage('Archival') {
   agent {
    docker {
     image 'node:latest'
     reuseNode true
    }
   }
   steps {
    zip zipFile: 'artifactory.zip' archive: true dir: '~/jenkins/artifact-repository​' glob: '**/**'
   } 
  }  
  stage('Deploy') {
    when {
      expression {
        currentBuild.result == null || currentBuild.result == 'SUCCESS' 
      }
    }
    steps {
        sh 'echo deploying...'
    }
   }
  }
}