pipeline {
    agent any

    stages {
        stage('Setup Python Environment') {
            steps {
                bat 'python -m venv venv'
                bat 'venv\\Scripts\\pip install -r requirements.txt'
            }
        }

        stage('Format Code with Black') {
            steps {
                bat 'venv\\Scripts\\black --diff --verbose . > black_formatting.log'
            }
        }

        stage('Run Tests') {
            steps {
                // Ajoute l'option --junitxml pour générer le fichier XML des résultats
                bat 'venv\\Scripts\\pytest --junitxml=results/test-results.xml'
            }
        }

        stage('Archive Test Results') {
            steps {
                // Archive le fichier XML des résultats de test
                archiveArtifacts artifacts: '**/test-results.xml', allowEmptyArchive: true
            }
        }

        stage('Archive Black Logs') {
            steps {
                // Archive les logs de Black
                archiveArtifacts artifacts: '**/black_formatting.log', allowEmptyArchive: true
            }
        }
    }
}
