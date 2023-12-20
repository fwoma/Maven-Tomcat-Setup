pipeline {
    agent any
    
    tools {
        // Assuming "maven" is the name of your Maven installation in Jenkins
        maven "maven"
    }
    
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
                    sh '/usr/share/maven/bin/mvn clean package'
                }
            }
        }

        stage('Unit Test') {
            steps {
                sh '/usr/share/maven/bin/mvn test'
            }
        }

        stage('Integration Test') {
            steps {
                sh '/usr/share/maven/bin/mvn verify -DskipUnitTests'
            }
        }

        stage('Checkstyle Code Analysis') {
            steps {
                sh '/usr/share/maven/bin/mvn checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }

        stage('SonarScanning') {
            steps {
                sh '/usr/share/maven/bin/mvn sonar:sonar'
            }
        }

        stage("Publish to Nexus Repository Manager") {
            steps {
                sh '/usr/share/maven/bin/mvn deploy'
            }
        }

        stage("Deploy it to Tomcat") {
            steps {
                script {
                    deploy adapters: [tomcat9(credentialsId: 'tomcat', path: '', url: 'http://5.161.219.34:8080')], contextPath: null, war: '**/*.war'
                }
            }
        }
    }
}
