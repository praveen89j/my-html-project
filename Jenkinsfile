pipeline {
    agent any

    environment {
        SONARQUBE = 'SonarQubeServer' // SonarQube server name as configured in Jenkins
        SONARQUBE_SCANNER_HOME = tool name: 'SonarQube Scanner', type: 'Tool' // Use SonarQube Scanner tool
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the HTML project code from your version control system (e.g., Git)
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/praveen89j/my-html-project.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis on the HTML project
                    withSonarQubeEnv('SonarQubeServer') { // SonarQube server name as configured in Jenkins
                        sh '''#!/bin/bash
                            $SONARQUBE_SCANNER_HOME/bin/sonar-scanner \
                            -Dsonar.projectKey=my-html-project \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://20.51.231.88:9000/  // Adjust this URL if your SonarQube server is different
                            -Dsonar.language=html  // Optional, to specify language as HTML
                        '''
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    // Wait for SonarQube Quality Gate result
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }
}
