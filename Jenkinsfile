pipeline {
    agent { label 'Slave1' }
    environment {
        VENV_DIR = 'venv'
        REQUIREMENTS_FILE = 'requirements.txt'
    }
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        
        stage('Setup Python Environment') {
            steps {
                script {
                    // Créer un environnement virtuel
                    bat 'python -m venv %VENV_DIR%'
                    
                    // Installer les dépendances
                    bat '"%VENV_DIR%\\Scripts\\pip" install -r %REQUIREMENTS_FILE%'
                }
            }
        }

        stage('Format Code with Black') {
            steps {
                script {
                    // Exécuter Black pour formater le code
                    bat '"%VENV_DIR%\\Scripts\\black" .'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Exécuter les tests
                    bat '"%VENV_DIR%\\Scripts\\pytest"'
                }
            }
        }

        stage('Archive Black Logs') {
            steps {
                script {
                    // Si tu veux archiver les logs de Black, tu peux le faire ici
                    // bat 'copy black_formatting.log some_destination_path' (à adapter)
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline terminé'
        }
        success {
            echo 'Le pipeline a réussi avec succès.'
        }
        failure {
            echo 'Le pipeline a échoué.'
        }
    }
}
