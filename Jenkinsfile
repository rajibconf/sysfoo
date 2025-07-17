pipeline {
  agent any
  tools {
    maven 'Maven 3.9.11'
  }
  stages {
    stage('Build') {
      steps {
        echo 'Building...'
        sh 'mvn -f pom.xml compile'
      }
    }
    stage('Test') {
      steps {
        echo 'Testing...'
        sh 'mvn -f pom.xml clean test'
      }
    }
    stage('Package') {
      steps {
        echo 'Truncating and Packaging...'
        sh '''
          GIT_SHORT_COMMIT=$(echo $GIT_COMMIT | cut -c 1-7)
          echo "Short commit: $GIT_SHORT_COMMIT"
    
          # Set the version to the short commit
          mvn versions:set -DnewVersion=$GIT_SHORT_COMMIT
          mvn versions:commit
    
          # Package the JAR
          mvn -f pom.xml package -DskipTests
        '''
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      }
    }
  }
}
