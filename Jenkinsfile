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
    
    stage ('Check-Git-Secrets') {
      steps {
      sh 'rm trufflehog || true'  
      sh 'docker run gesellix/trufflehog --json https://github.com/devsecopsst/webapp2.git > trufflehog'
      sh 'cat trufflehog'
      }
    }
    
    stage ('Source composition Analysis'){
      steps{
        sh 'rm owasp* || true'
        sh 'wget https://raw.githubusercontent.com/devsecopsst/webapp2/master/owasp-dependency-check.sh'
        sh 'chmod +x owasp-dependency-check.sh'
        sh 'bash owasp-dependency-check.sh'
        sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
      }
    }
    
    stage ('SAST'){
      steps {
        withSonarQubeEnv('sonar'){
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
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
  }
}
