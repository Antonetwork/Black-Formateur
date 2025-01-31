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
                // L'option --diff va afficher les changements (non réécrits directement dans le code)
                // --verbose permet d'inclure plus d'informations dans les logs
            }
        }

        stage('Run Tests') {
            steps {
                bat 'venv\\Scripts\\pytest'
            }
        }

        stage('Archive Test Results') {
            steps {
                archiveArtifacts artifacts: '**/test-results.xml', allowEmptyArchive: true
            }
        }

        stage('Archive Black Logs') {
            steps {
                archiveArtifacts artifacts: '**/black_formatting.log', allowEmptyArchive: true
            }
        }
    }
}
