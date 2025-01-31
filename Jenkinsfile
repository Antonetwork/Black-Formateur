pipeline {
    agent { label 'Slave1' }  // Utilise le Slave1 pour l'exécution du pipeline

    stages {
        stage('Setup Python Environment') {
            steps {
                script {
                    // Assurez-vous que l'environnement virtuel est installé sur Slave1
                    if (!fileExists('venv')) {
                        sh 'python -m venv venv'  // Crée un environnement virtuel si nécessaire
                    }
                    sh 'venv/Scripts/pip install -r requirements.txt'  // Installe les dépendances
                }
            }
        }

        stage('Format Code with Black') {
            steps {
                // Exécute Black sur tous les fichiers Python du projet et modifie les fichiers en place
                bat 'venv\\Scripts\\black . > black_formatting.log'  // Formate les fichiers Python
            }
        }

        stage('Archive Formatted Code') {
            steps {
                // Archive les fichiers Python formatés après l'exécution de Black
                archiveArtifacts artifacts: '**/*.py', allowEmptyArchive: true
            }
        }
    }

    post {
        always {
            // Si nécessaire, nettoyez les fichiers ou faites d'autres actions après chaque exécution
            echo 'Pipeline terminé'
        }
    }
}
