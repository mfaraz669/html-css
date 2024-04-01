pipeline {
    agent any {

        stages {
            stage('Checkout') {
                steps {
                    gitCheckout (
                        branch: "main",
                        url: "https://github.com/mfaraz669/html-css.git"
                    )
                        
                }
            }

            stage('Install Dependencies') {
                steps {
                    nodejs('21.5.0') {
                        sh 'npm install'
                    }
                }
            }

            stage('Build') {
                steps {
                    // Build the application
                    nodejs('21.5.0')
                    sh 'npm run build'
                }
            }

            stage('Static code analysis') {
                environment {
                    SONAR_URL = "http://100.26.226.15:9000"
                }
                steps {
                    withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
                        //RUN SONARQUBE TO ANALYSE CODE
                    withSonarQubeEnv('SonarQube-Server') {
                        sh 'sonar-scanner' -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}}
                    }
                }
            }
            
            stage("Quality Gate") {
                steps {
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate(abortPipeline: true)
                    }
                }
            }
            
        }
            
    }


        
}
