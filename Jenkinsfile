pipeline {
    agent any
    environment {
        path = "/opt/apache-maven-3.9.5/bin:$PATH"
    }
    stages {
        stage("clone code") {
            steps {
               git branch: "main", url: "https://github.com/mohithnaidu03/maven.git"
            }
        } 
        stage("build code"){
            steps {
                sh "mvn clean install"
        
             }
          }
          stage("s3 upload build") {
              steps {
                  sh "aws s3 cp /var/lib/jenkins/workspace/pipeline-build/webapp/ s3://backup-shell1 --recursive"
              }
          }
        stage("Deploy DEV") {
        	steps {
            	sshagent(['ec2-user-1']) {
                	sh "scp -o StrictHostKeyChecking=no webapp/target/webapp.war ec2-user@172.31.40.72:/opt/apache-tomcat-9.0.82/webapps"
            	}
            }   
         }  
    }
}
