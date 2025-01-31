pipeline {
    agent { label 'Slave1' } // Assure-toi que ce pipeline s'exécute uniquement sur Slave1

    stages {
        stage('Setup Python Environment') {
            steps {
                script {
                    // Créer un environnement virtuel Python
                    bat 'python -m venv venv'
                    // Installer les dépendances Python depuis requirements.txt
                    bat 'venv\\Scripts\\pip install -r requirements.txt'
                }
            }
        }

        stage('Format Code with Black') {
            steps {
                // Exécuter Black et rediriger les logs vers un fichier local sur Slave1
                bat 'venv\\Scripts\\black --diff --verbose . > C:\\jenkins\\workspace\\Black formateur\\output\\black_formatting.log'
            }
        }

        stage('Run Tests') {
            steps {
                // Exécuter les tests avec pytest et générer un fichier XML pour les résultats
                bat 'venv\\Scripts\\pytest --junitxml=C:\\jenkins\\workspace\\Black formateur\\output\\results\\test-results.xml'
            }
        }

        stage('Archive Test Results') {
            steps {
                // Archive le fichier XML des résultats de test
                archiveArtifacts artifacts: 'C:\\jenkins\\workspace\\Black formateur\\output\\results\\test-results.xml', allowEmptyArchive: true
            }
        }

        stage('Archive Black Logs') {
            steps {
                // Archive les logs de Black
                archiveArtifacts artifacts: 'C:\\jenkins\\workspace\\Black formateur\\output\\black_formatting.log', allowEmptyArchive: true
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
