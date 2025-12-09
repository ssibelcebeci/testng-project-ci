pipeline {

    agent any

    options {
        timestamps()
        disableConcurrentBuilds()
        buildDiscarder(logRotator(
            numToKeepStr: '20',
            artifactNumToKeepStr: '10'
        ))
    }

    environment {
        ALLURE_RESULTS_PATH = 'target/allure-results'
        RECIPIENTS          = 'sibellovski@gmail.com'
    }

    tools {
        jdk    'JDK24'
        maven  'Maven-3.9'
    }

    stages {

        stage('Prepare Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ssibelcebeci/testng-project-ci.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn -U -B clean test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Generate Allure Report') {
            steps {
                allure([
                    commandLine: 'Allure',
                    results: [[path: "${ALLURE_RESULTS_PATH}"]]
                ])
            }
        }

        stage('Zip Allure Results') {
            steps {
                sh 'rm -f allure-results.zip || true'
                sh "zip -r allure-results.zip ${ALLURE_RESULTS_PATH}"
                archiveArtifacts artifacts: 'allure-results.zip', fingerprint: true
            }
        }
    }

    post {

        success {
            emailext(
                subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
Job: ${env.JOB_NAME}
Build: ${env.BUILD_NUMBER}
Result: SUCCESS

Allure Report:
${env.BUILD_URL}allure
""",
                to: "${RECIPIENTS}",
                attachmentsPattern: 'allure-results.zip'
            )
        }

        failure {
            emailext(
                subject: "FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
Job: ${env.JOB_NAME}
Build: ${env.BUILD_NUMBER}
Result: FAILED

Details:
${env.BUILD_URL}
""",
                to: "${RECIPIENTS}",
                attachLog: true,
                attachmentsPattern: 'allure-results.zip'
            )
        }

        always {
            echo "Build finished. Result: ${currentBuild.currentResult}"
        }
    }
}
