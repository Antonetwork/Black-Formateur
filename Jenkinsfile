pipeline {
    agent { label 'Slave1' }  // Utiliser le slave Windows avec ce label

    environment {
        PYTHON_ENV = 'venv'  // Nom de l'environnement virtuel Python
        TEST_RESULTS_DIR = 'C:\\Jenkins\\test_results'  // Répertoire pour les résultats des tests
        BLACK_LOG_DIR = 'C:\\Jenkins\\black_formatter'  // Nouveau répertoire pour stocker les logs de Black
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm  // Récupère le code source depuis le dépôt Git
            }
        }

        stage('Setup Python Environment') {
            steps {
                script {
                    // Créer et activer un environnement virtuel Python
                    bat 'python -m venv %PYTHON_ENV%'  // Crée un venv
                    bat '%PYTHON_ENV%\\Scripts\\pip install -r requirements.txt'  // Installe les dépendances, y compris Black
                }
            }
        }

        stage('Format Code with Black') {
            steps {
                script {
                    // Formater le code Python avec Black et stocker les logs dans un fichier spécifique
                    bat '%PYTHON_ENV%\\Scripts\\black . > %BLACK_LOG_DIR%\\black_formatting.log 2>&1'
                    // `2>&1` redirige les erreurs vers le même fichier de log
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Exécuter pytest et générer un rapport XML dans le répertoire de résultats
                    bat '%PYTHON_ENV%\\Scripts\\pytest --maxfail=1 --disable-warnings -q --junitxml=%TEST_RESULTS_DIR%\\test-results.xml'
                }
            }
        }

        stage('Archive Test Results') {
            steps {
                // Stocker les résultats dans Jenkins
                archiveArtifacts artifacts: '%TEST_RESULTS_DIR%\\test-results.xml', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            echo "Les résultats sont stockés sur le Slave dans %TEST_RESULTS_DIR%\\test-results.xml"
            echo "Les logs de Black sont disponibles dans %BLACK_LOG_DIR%\\black_formatting.log"
        }
    }
}
