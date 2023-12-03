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
                sh 'mvn sonar:sonar 
            }
			}
        
      stage("Publish to Nexus Repository Manager") {
            steps {
            sh 'mvn deploy'
            }
            }
      stage("Deploy it to tomcat") {
            steps {
            deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://172.31.21.100:8000')], contextPath: null, war: '**/*.war'
            }
        }
}
}
