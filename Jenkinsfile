pipeline {

    agent any

    tools {
        jdk 'JDK24'
        maven 'Maven-3.9'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ssibelcebeci/testng-project-ci.git'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('Generate Allure Report') {
            steps {
                sh '''
                    allure generate target/allure-results -o target/allure-report --clean
                '''
            }
        }
    }

    post {
        always {
            publishHTML(target: [
                reportDir: 'target/allure-report',
                reportFiles: 'index.html',
                reportName: 'Allure Report'
            ])
        }
    }
}
