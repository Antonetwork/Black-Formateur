pipeline {
    agent { label 'Slave1' } // Exécuter sur Slave1
    environment {
        VENV_DIR = 'venv' // Dossier pour l'environnement virtuel
        REQUIREMENTS_FILE = 'requirements.txt' // Fichier des dépendances
    }
    stages {
        // Étape pour récupérer le code source depuis Git
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        // Étape pour configurer l'environnement Python
        stage('Setup Python Environment') {
            steps {
                script {
                    // Créer l'environnement virtuel Python
                    bat 'python -m venv %VENV_DIR%'
                    
                    // Installer les dépendances Python depuis le fichier requirements.txt
                    bat '"%VENV_DIR%\\Scripts\\pip" install -r %REQUIREMENTS_FILE%'
                }
            }
        }

        // Étape pour formater le code avec Black
        stage('Format Code with Black') {
            steps {
                script {
                    // Exécuter Black pour formater le code
                    bat '"%VENV_DIR%\\Scripts\\black" .'
                }
            }
        }

        // Étape pour exécuter les tests avec pytest
        stage('Run Tests') {
            steps {
                script {
                    // Exécuter les tests avec pytest
                    bat '"%VENV_DIR%\\Scripts\\pytest"'
                }
            }
        }

        // Étape pour archiver les logs de Black (si nécessaire)
        stage('Archive Black Logs') {
            steps {
                script {
                    // Exemple pour archiver les logs de Black
                    // bat 'copy black_formatting.log some_destination_path'
                }
            }
        }
    }
    post {
        // Actions à réaliser après le pipeline, quelle que soit l'issue
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
