// Jenkinsfile
pipeline {
    agent any // Le pipeline peut s'exécuter sur n'importe quel agent disponible (votre serveur Jenkins sur Linux)

    stages {
        stage('Checkout du Code') {
            steps {
                // Cette étape est souvent implicite, mais la définir est plus clair.
                echo 'Clonage du dépôt GitHub...'
                // L'agent Jenkins clone automatiquement le code dans son espace de travail
            }
        }
        stage('Exécution du Script') {
            steps {
                // Utilisation de 'sh' pour exécuter des commandes shell Linux
                sh '''
                echo "Démarrage de l'environnement Linux pour le script..."
                # La commande python3 doit être disponible sur le serveur Jenkins
                python3 hello.py
                echo "Le script a terminé."
                '''
            }
        }
    }
    post {
        // Actions après la fin du pipeline, peu importe le résultat
        always {
            echo 'Pipeline terminé.'
        }
        success {
            echo 'Notification: Le build a réussi ! 🎉'
        }
        failure {
            echo 'Notification: Le build a échoué ! 😢'
        }
    }
}