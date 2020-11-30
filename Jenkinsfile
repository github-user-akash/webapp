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
      
 stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /home/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
      }
    }
     stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
      }
    }
  stage ('DAST') 
    {
 steps {
 sh 'rm /var/lib/jenkins/workspace/demo1/testreport.xml || true'
 sh ' docker run --user root -v $(pwd):/zap/wrk/ --rm -t owasp/zap2docker-weekly zap-baseline.py -t http://testphp.vulnweb.com -g gen.conf -x testreport.xml || true' 
 sh 'cat /var/lib/jenkins/workspace/demo1/testreport.xml'
 }
 }
  }}
