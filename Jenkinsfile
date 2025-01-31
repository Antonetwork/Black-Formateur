pipeline {
    agent { label 'Slave1' }  // Assure-toi que ce pipeline s'exécute uniquement sur Slave1

    stages {
        stage('Setup Python Environment') {
            steps {
                bat 'python -m venv venv'
                bat 'venv\\Scripts\\pip install -r requirements.txt'
            }
        }

        stage('Format Code with Black') {
            steps {
                // Rediriger les logs de Black vers un répertoire local sur Slave1
                bat 'venv\\Scripts\\black --diff --verbose . > C:\\jenkins\\workspace\\Black formateur\\output\\black_formatting.log'
            }
        }

        stage('Run Tests') {
            steps {
                // Ajouter l'option --junitxml pour générer les résultats des tests localement sur Slave1
                bat 'venv\\Scripts\\pytest --junitxml=C:\\jenkins\\workspace\\Black formateur\\output\\results\\test-results.xml'
            }
        }

        stage('Archive Test Results') {
            steps {
                // Archive les résultats des tests localement sur Slave1, mais dans un répertoire sous 'output'
                archiveArtifacts artifacts: 'C:\\jenkins\\workspace\\Black formateur\\output\\results\\test-results.xml', allowEmptyArchive: true
            }
        }

        stage('Archive Black Logs') {
            steps {
                // Archive les logs de Black localement sur Slave1
                archiveArtifacts artifacts: 'C:\\jenkins\\workspace\\Black formateur\\output\\black_formatting.log', allowEmptyArchive: true
            }
        }
    }
}
