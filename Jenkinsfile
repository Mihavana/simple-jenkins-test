// Jenkinsfile (Corrigé - Ajout de l'option skipDefaultCheckout)
pipeline {
    agent none

    options {
        // AJOUTER CETTE LIGNE : Indique à Jenkins de ne pas faire le checkout implicite
        skipDefaultCheckout() 
    }

    stages {
        
        // NOUVELLE PREMIÈRE ÉTAPE : Fait le checkout manuellement et proprement
        stage('Checkout SCM') {
            agent any // Utilise le nœud par défaut pour le clonage
            steps {
                // Fait un checkout explicite SANS conflit
                checkout scm
                echo "Code cloné et prêt."
            }
        }
        
        // 2. Étape Docker (Préparer l'environnement Docker)
        stage('Préparer l\'environnement Docker') {
            agent {
                docker {
                    image 'docker:stable-cli'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            // ... (le reste de l'étape inchangé)
            steps {
                echo "Vérification de Docker..."
                sh 'docker inspect -f . python:3.9-slim' 
                sh 'docker pull python:3.9-slim'
            }
        }
        
        // 3. Étape Python
        stage('Exécution du Script Python') {
            // ... (le reste de l'étape inchangé)
            agent {
                docker {
                    image 'python:3.9-slim'
                    args '-u root'
                }
            }
            steps {
                sh '''
                echo "Démarrage de l\'exécution du script Python..."
                python3 hello.py
                echo "Le script a terminé."
                '''
            }
        }
    }
}