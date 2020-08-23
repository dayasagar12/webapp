pipeline {  
  agent any
  tools {
  maven 'Maven'
  
  }

   stages {
     stage('Initiliaze') {
     
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
        sh 'docker run gesellix/trufflehog --json https://github.com/dayasagar12/webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
     
     
   stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
     
    stage ('Deploy-To-Tomcat') {
      steps {
      sshagent(['tomcat']) {
       sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@13.232.42.45:/prod/apache-tomcat-8.5.57/webapps/webapp.war'
              }      
           }
    }
   }
}
