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
    stage ('Build') {
      steps {
      sh 'mvn clean package'
      }
    }
     stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@3.92.205.207:/prod/apache-tomcat-8.5.65/webapps/webapp.war'
              }      
           }       
    }
    
   }
    
}
