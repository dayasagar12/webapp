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
        sh 'docker run gesellix/trufflehog --json https://github.com/dayasagar12/webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    
    
     stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/dayasagar12/webapp/master/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
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
   
    stage ('DAST') {
      steps {
        sshagent(['zap']) {
         sh 'ssh -o  StrictHostKeyChecking=no ubuntu@13.232.158.44 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://13.232.202.25:8080/webapp/" || true'
        }
      }
    }
    
  }
    
}
