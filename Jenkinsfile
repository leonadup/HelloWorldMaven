pipeline {
    agent any

    environment {
        // Utilisation du credential Git dans Jenkins pour accéder au dépôt
        GIT_CREDENTIALS = 'tokenJenkins'  // Le nom du credential dans Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout du code depuis le repository Git
                git credentialsId: "${GIT_CREDENTIALS}", url: 'https://github.com/leonadup/HelloWorldMaven.git'
            }
        }

        stage('Build') {
            steps {
                // Exécution de la commande Maven pour nettoyer et construire le projet
                sh 'mvn clean install'
            }
        }
    }

    post {
        success {
            script {
                

                // Créer un tag avec un format de type V-2.<build_number>
                def tagName = "V-3.${BUILD_NUMBER}"

                // Récupérer les informations du credential Git dans Jenkins
                withCredentials([usernamePassword(credentialsId: "${GIT_CREDENTIALS}", usernameVariable: 'GIT_USER', passwordVariable: 'GIT_TOKEN')]) {
                    // Formater l'URL de dépôt Git en utilisant les variables pour le username et le token
                    def gitUrl = "https://${GIT_USER}:${GIT_TOKEN}@github.com/leonadup/HelloWorldMaven.git"

                    // Créer et pousser le tag sur le dépôt Git
                    sh """
                        git tag ${tagName}
                        git push ${gitUrl} ${tagName}
                    """
                }
            }
        }

        failure {
            echo 'Le build a échoué. Aucun tag n\'a été créé.'
        }
    }
}

