pipeline {
  agent any 
  tools {
    maven 'Maven'
  } 
    stage ('Builds') {
      steps {
      sh 'mvn clean package'
      }
    }
  }
}
