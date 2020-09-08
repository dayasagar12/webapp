pipeline {  
  agent any


   stages {
     stage('Initiliaze') {
     
      steps {
       sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            '''
      }
    }    
     
     
     stage ('sonar') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
      }
    }
     
   stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
     
   }
}
