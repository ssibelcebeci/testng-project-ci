pipeline {

    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven-3.9'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ssibelcebeci/testng-project-ci.git'
            }
        }

        stage('Run Test') {
            steps {
                sh 'mvn clean te
