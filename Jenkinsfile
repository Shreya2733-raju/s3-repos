pipeline {
    agent any
    
    tools {
        maven 'Maven'
    }

    environment {
        S3_BUCKET = "tom-cat-bucket"
        WAR_FILE = "target/*.war"
        EC2_IP = "13.60.57.99"
        KEY = "testkey.pem"
    }

    stages {

        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Shreya2733-raju/Sampleapp.git'
            }
        }

        stage('Build WAR') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Upload to S3') {
            steps {
                bat 'aws s3 cp %WAR_FILE% s3://%S3_BUCKET%/'
            }
        }

        stage('Deploy to EC2 Tomcat') {
            steps {
                bat '''
                aws s3 cp s3://%S3_BUCKET%/app.war .

                scp -i %KEY% app.war ubuntu@%EC2_IP%:/var/lib/tomcat10/webapps/

                ssh -i %KEY% ubuntu@%EC2_IP% "sudo systemctl restart tomcat10"
                '''
            }
        }
    }
}
