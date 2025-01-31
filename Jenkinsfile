pipeline {
    agent any
    agent { label 'Slave1' }  // Assure-toi que ce pipeline s'exécute uniquement sur Slave1

    stages {
        stage('Setup Python Environment') {
@@ -11,28 +11,29 @@ pipeline {

        stage('Format Code with Black') {
            steps {
                bat 'venv\\Scripts\\black --diff --verbose . > black_formatting.log'
                // Rediriger les logs de Black vers un répertoire local sur Slave1
                bat 'venv\\Scripts\\black --diff --verbose . > C:\\jenkins\\workspace\\Black formateur\\output\\black_formatting.log'
            }
        }

        stage('Run Tests') {
            steps {
                // Ajoute l'option --junitxml pour générer le fichier XML des résultats
                bat 'venv\\Scripts\\pytest --junitxml=results/test-results.xml'
                // Ajouter l'option --junitxml pour générer les résultats des tests localement sur Slave1
                bat 'venv\\Scripts\\pytest --junitxml=C:\\jenkins\\workspace\\Black formateur\\output\\results\\test-results.xml'
            }
        }

        stage('Archive Test Results') {
            steps {
                // Archive le fichier XML des résultats de test
                archiveArtifacts artifacts: '**/test-results.xml', allowEmptyArchive: true
                // Archive les résultats des tests localement sur Slave1, mais dans un répertoire sous 'output'
                archiveArtifacts artifacts: 'C:\\jenkins\\workspace\\Black formateur\\output\\results\\test-results.xml', allowEmptyArchive: true
            }
        }

        stage('Archive Black Logs') {
            steps {
                // Archive les logs de Black
                archiveArtifacts artifacts: '**/black_formatting.log', allowEmptyArchive: true
                // Archive les logs de Black localement sur Slave1
                archiveArtifacts artifacts: 'C:\\jenkins\\workspace\\Black formateur\\output\\black_formatting.log', allowEmptyArchive: true
            }
        }
    }
