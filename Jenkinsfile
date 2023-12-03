pipeline {
    agent any
    environment {
        PATH = "/opt/apache-maven-3.9.6/bin:${PATH}"
    }
    stages {
        stage('Clone Code') {
            steps {
                git 'https://github.com/architectdevops7/devops-security.git'
            }
        }
        stage('Build Package') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Sonar Analysis') {
            steps {
                script {
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn sonar:sonar -Dsonar.host.url=http://44.203.122.221:9000 -Dsonar.login=1b2f4037b10bc2fed85a18a4a03bc18bcdbc28b3'
                    }
                }
            }
        }
    }
}
