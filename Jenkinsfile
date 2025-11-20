// Jenkinsfile (Corrigé - Suppression de l'étape Checkout redondante)
pipeline {
    // Agent au niveau global défini à 'none'
    agent none

    options {
        // AJOUTEZ CETTE OPTION
        // Cette étape utilise l'étape Pipeline "cleanWs" pour nettoyer avant ou après l'exécution.
        // Ici, nous faisons le nettoyage au début.
        skipDefaultCheckout() // Optionnel, évite un checkout implicite non désiré
        buildDiscarder(logRotator(daysToKeepStr: '30', numToKeepStr: '10')) // Exemple de conservation des logs
    }

    stages {
        
        // Supprimez le stage('Checkout SCM') qui était redondant et causait l'erreur "not in a git directory".
        // Le code est déjà disponible dans le workspace à partir d'ici.
        
        // 1. Étape Docker (pour pull/inspect, etc.)
        stage('Préparer l\'environnement Docker') {
            agent {
                docker {
                    // Utilise l'image officielle du client Docker (inclut le binaire 'docker')
                    image 'docker:stable-cli'
                    // LIGNE CRUCIALE : Passe le socket de l'hôte à ce conteneur agent (DooD)
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
        
        // 2. Étape Python (pour exécuter votre script)
        stage('Exécution du Script Python') {
            agent {
                docker {
                    // Utilise l'image Python
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