pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout your code
                git 'https://your-repo-url.git'
            }
        }

        stage('Run Postman Tests') {
            steps {
                script {
                    docker.image('newman-allure-html').inside('-v $WORKSPACE:/etc/newman') {
                        sh 'newman run /etc/newman/your-collection.json -e /etc/newman/your-environment.json -r allure,html --reporter-allure-export /etc/newman/allure-results --reporter-html-export /etc/newman/newman-report.html'
                    }
                }
            }
        }

        stage('Generate Allure Report') {
            steps {
                script {
                    allure([
                        includeProperties: false,
                        jdk: '',
                        properties: [],
                        reportBuildPolicy: 'ALWAYS',
                        results: [[path: 'allure-results']]
                    ])
                }
            }
        }

        stage('Archive HTML Report') {
            steps {
                archiveArtifacts artifacts: 'newman-report.html', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            // Clean up workspace after the build
            cleanWs()
        }
    }
}
