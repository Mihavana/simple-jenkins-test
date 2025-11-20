// Jenkinsfile (Modifié)
pipeline {
    // 1. Définir un agent 'none' au niveau global
    agent none

    stages {
        stage('Checkout SCM') {
            // Utiliser le nœud par défaut pour la phase de checkout
            agent any
            steps {
                // Le checkout est géré automatiquement par Jenkins avant cette étape
                script {
                    echo "Code récupéré."
                }
            }
        }
        
        // 2. Étape Docker (pour pull/inspect, etc.)
        stage('Préparer l\'environnement Docker') {
            agent {
                docker {
                    // Utiliser l'image officielle du client Docker (inclut le binaire 'docker')
                    image 'docker:stable-cli'
                    // LIGNE CRUCIALE : Passer le socket de l'hôte à ce conteneur agent
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                echo "Vérification de Docker..."
                // Ces commandes vont fonctionner car l'exécutable 'docker' et le socket sont disponibles
                sh 'docker inspect -f . python:3.9-slim' 
                sh 'docker pull python:3.9-slim'
            }
        }
        
        // 3. Étape Python (pour exécuter votre script)
        stage('Exécution du Script Python') {
            agent {
                docker {
                    // Utiliser l'image Python, sans besoin de Docker ici
                    image 'python:3.9-slim'
                    args '-u root'
                }
            }
            steps {
                // Toutes les commandes 'sh' s'exécutent dans l'agent python:3.9-slim
                sh '''
                echo "Démarrage de l\'exécution du script Python..."
                python3 hello.py
                echo "Le script a terminé."
                '''
            }
        }
    }
}