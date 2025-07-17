pipeline {
  agent any
  tools {
    maven 'Maven 3.6.1'
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
        echo 'Truncating...'
        # Truncate the GIT_COMMIT to the first 7 characters
        sh 'GIT_SHORT_COMMIT=$(echo $GIT_COMMIT | cut -c 1-7)'
        # Set the version using Maven
        sh 'mvn versions:set -DnewVersion="$GIT_SHORT_COMMIT"'
        sh 'mvn versions:commit'

        echo 'Packaging...'
        sh 'mvn -f pom.xml package -DskipTests'
        archiveArtifacts artifacts: '**/target/*.jar', fingerprints: true
      }
    }
  }
}
