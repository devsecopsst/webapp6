pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
  } 
    
    stage ('Check-Git-Secrets') {
      steps {
      sh 'rm trufflehog || true'  
      sh 'docker run gesellix/trufflehog --json https://github.com/devsecopsst/webapp4.git > trufflehog'
      sh 'cat trufflehog'
      }
    }

    stage ('Builds') {
      steps {
      sh 'mvn clean package'
      }
    }
    stage('Deploy-To-Tomcat'){
      steps{
        sshagent(['tomcat']){
        sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@18.191.222.132:/prod/apache-tomcat-8.5.50/webapps/webapp.war'
        }
      }
     } 
   
    }
