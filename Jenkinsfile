// Jenkinsfile (Structure la plus simple pour le SCM)
pipeline {
    agent none
    
    // Définition de la source de code
    options {
        // Empêche tout autre tentative de checkout implicite
        skipDefaultCheckout(true) 
    }

    stages {
        
        // NOUVEAU PREMIER STAGE : CLONAGE MANUEL
        stage('Clonage SCM') {
            agent any
            steps {
                // Utilise le step 'git' avec les paramètres explicites. 
                // Assurez-vous que l'ID d'identifiants est correct.
                git branch: 'main', 
                    credentialsId: 'github-ssh', 
                    url: 'git@github.com:Mihavana/simple-jenkins-test.git'
                echo "Code cloné et prêt dans le workspace."
            }
        }
        
        // 2. Étape Docker (Préparer l\'environnement Docker)
        stage('Préparer l\'environnement Docker') {
            agent {
                docker {
                    image 'docker'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                echo "Vérification de Docker..."
                sh 'docker inspect -f . python:3.9-slim' 
                sh 'docker pull python:3.9-slim'
            }
        }
        
        // 3. Étape Python
        stage('Exécution du Script Python') {
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