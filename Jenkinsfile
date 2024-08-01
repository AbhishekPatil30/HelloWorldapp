pipeline {
    agent any
    tools {
        maven 'Maven 3.8.7'
    }
    environment {
      TOMCAT_USER = "ec2-user"
      TOMCAT_HOST  = "http://3.110.183.84:9050/"
      TOMCAT_PATH = "/opt/tomcat/apache-tomcat-9.0.68/webapps"
}
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'gittoken', url: 'https://github.com/AbhishekPatil30/HelloWorld.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo "Archiving the Artifacts"
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['tomcat']) {
                    sh "rsync -avz -e 'ssh -o StrictHostKeyChecking=no' --delete target/*.war ${TOMCAT_USER}@${TOMCAT_HOST}:${TOMCAT_PATH}"
                }
            }
        }
    }
}
