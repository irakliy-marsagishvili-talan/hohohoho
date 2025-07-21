pipeline {
    agent any
    environment {
        SONAR_SCANNER_OPTS = "-Xmx512m"
    }
    stages {
        stage('Build and Test') {
            steps {
                dir('') {
                    sh 'chmod +x mvnw'
                    sh './mvnw clean install'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                dir('') {
					withCredentials([string(credentialsId: 'sonar_token', variable: 'SONAR_TOKEN')]) {
						withSonarQubeEnv("SonarQube") {
							sh './mvnw -e sonar:sonar -Dsonar.projectKey=bookservice -Dsonar.login=${SONAR_TOKEN}'		
						}				
					}
				}
            }
        }
    }
    post {
        always {
            dir('') {
                junit '**/target/surefire-reports/*.xml'
            }
        }
    }
}