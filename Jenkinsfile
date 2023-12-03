pipeline {
    agent any
    stages {
        stage("Clone code from GitHub") {
            steps {
                script {
                    git url: 'https://github.com/fwoma/Maven-Tomcat-Setup.git', branch: 'main'
                }
            }
        }
        stage("Maven Build") {
            steps {
                script {
                    sh "mvn clean package"
                }
            }
        }
        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Integration Test') {
            steps {
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage('Checkstyle Code Analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }
      stage('SonarScanning') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.host.url=http://172.31.20.208:9000 -Dsonar.login=a597aa0c2310906aa3bfba683e3bdc424f0293f3'
            }
			}
        
      stage("Publish to Nexus Repository Manager") {
            steps {
            sh 'mvn clean deploy'
            }
            }
      stage("Deploy it to tomcat") {
            steps {
            sh 'ssh -t -t root@3.85.175.67 -o StrictHostKeyChecking=no "mvn install tomcat7:deploy"'
            }
        }
}
}
