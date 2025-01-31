pipeline {
    agent { label 'Slave1' }  // Utilise le Slave1 pour l'exécution du pipeline

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Setup Python Environment') {
            steps {
                script {
                    // Crée l'environnement virtuel si nécessaire
                    if (!fileExists('venv')) {
                        bat 'python -m venv venv'  // Crée un environnement virtuel
                    }
                    bat 'venv\\Scripts\\pip install -r requirements.txt'  // Installe les dépendances
                }
            }
        }

        stage('Format Code with Black') {
            steps {
                // Exécute Black pour formater le code Python
                bat 'venv\\Scripts\\black . > black_formatting.log'  // Utilisation de `bat` pour Windows
            }
        }

        stage('Archive Formatted Code') {
            steps {
                // Archive les fichiers Python formatés
                archiveArtifacts artifacts: '**/*.py', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            // Actions à effectuer après chaque exécution du pipeline
            echo 'Pipeline terminé'
        }
    }
}
