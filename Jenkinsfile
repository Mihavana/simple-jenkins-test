// Jenkinsfile
pipeline {
    // Définir l'agent du pipeline comme une image Docker (python:3.9-slim)
    agent {
        docker {
            image 'python:3.9-slim' // L'image qui contient Python
            args '-u root' // Optionnel, pour éviter les problèmes de permissions utilisateur
        }
    }

    stages {
        stage('Exécution du Script') {
            steps {
                // Maintenant, toutes les commandes 'sh' s'exécutent DANS le conteneur python:3.9-slim
                sh '''
                echo "Démarrage de l'exécution du script Python directement dans l'environnement Python..."
                python3 hello.py
                echo "Le script a terminé."
                '''
            }
        }
    }
    // ... (Section post non modifiée)
}