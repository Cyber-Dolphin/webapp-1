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
        sh 'docker run gesellix/trufflehog --json https://github.com/Cyber-Dolphin/webapp-1.git > trufflehog' 
        sh 'cat trufflehog' 
      }
    }
    
    stage ('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    
    stage ('Deploy') {
      steps {
        sshagent(['tomcat']) {
          sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@3.129.244.116:/prod/apache-tomcat-10.0.7/webapps/webapp.war'
        }
      }
    }
    
  }
}
