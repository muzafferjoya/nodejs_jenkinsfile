pipeline {
  agent any  
  stages {
    stage('Install Packages') {
      steps {
        sh 'npm install'
      }
    }
    stage('Test and Build') {
      parallel {
        stage('Run Tests') {
          steps {
            sh 'npm run test'
          }
        }
        stage('Create Build Artifacts') {
          steps {
            sh 'npm run build'
          }
        }
      }
    }
    stage('Deployment') {
      parallel {
        stage('Staging') {
          when {
            branch 'master'
          }
          steps {
            withAWS(region:'us-east-1',credentials:'c6-0c386a4ca41f') {
              
              s3Upload(bucket: 'ront', workingDir:'build', includePathPattern:'**/*');
            }
            mail(subject: 'Staging Build', body: 'New Deployment to Staging', to: 'muzaffar@gmail.com')
          }
        }
        
      }
    }
  }
}
