pipeline {

    agent any  // bu pipeline dosyasina her hangi bir özel agent eklemedik

// jenkins icerisinde jdk ve maven i tanimladik. bu sayede localdeki jdk ve maven a bakmayacak.
    tools {
        jdk 'JDK24'
        maven 'Maven-3.9'
    }

// stages sirayla jenkins in calistiracagi komutlar
    stages {

// ilk adimda github tan projeyi cekip, main branche checkout oluyor
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ssibelcebeci/testng-project-ci.git'
            }
        }

// 2. stepte testleri calistiriyor
        stage('Run Tests') {
            steps {
            //MAC
                sh 'mvn clean test'
            // Windows
            //bat 'mvn clean test'
            }
        }

        // bu stepte ise reportu olusturuyor
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
                // allure result un olustugu klasörü tanimladik
                reportDir: 'target/allure-report',
                reportFiles: 'index.html',
                reportName: 'Allure Report'
            ])
        }
    }
}
