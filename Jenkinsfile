pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Cloner le dépôt
                git 'https://github.com/Mohamed-KBIBECH/DevSecOps.git'
            }
        }
        stage('Build') {
            steps {
                // Construire le projet Maven
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                // Exécuter les tests
                sh 'mvn test'
            }
        }
        stage('SCA with Dependency-Check') {
            steps {
                echo 'Analyse de la composition des sources avec OWASP Dependency-Check...'
                bat '"<path vers denpendency check>\\dependency-check-10.0.2-release\\dependency-check\\bin\\dependency-check.bat" --project "demo" --scan . --format HTML --out dependency-check-report.html --nvdApiKey 181c8fc5-2ddc-4d15-99bf-764fff8d50dc --disableAsse'
            }
        }
            stage('ZAP Security Scan') {
            steps {
            script {
            def appUrl = 'https://ed3a-105-73-96-62.ngrok-free.app'
            bat "curl \"http://localhost:8081/JSON/spider/action/scan/?url=${appUrl}&recurse=true\"
            sleep(60)
            bat "curl \"http://localhost:8081/JSON/ascan/action/scan/?url=${appUrl}&recurse=true&in
            sleep(300)
            }
            }
            post {
            always {
            bat 'curl "http://localhost:8081/OTHER/core/other/htmlreport/" > zap_report1.html'
            }
            }
            }

        stage('Deploy') {
            steps {
                // Déploiement (peut être remplacé par votre stratégie de déploiement)
                sh 'echo "Déploiement de l\'application..."'
            }
        }
    }

    post {
        always {
            // Archive les fichiers importants à la fin
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
        }
        success {
            // Notifier en cas de succès
            echo 'Le build a réussi !'
        }
        failure {
            // Notifier en cas d'échec
            echo 'Le build a échoué.'
        }
    }
}
