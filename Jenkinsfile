pipeline {
    agent any
    
    environment {
        GIT_REPO_URL = 'git@github.com:GevireddyOffcial/SmartHotelApp-Infra.git'  // Replace with your Git repository URL
        GIT_BRANCH = 'main'  // Replace with the branch you want to push to
        CREDENTIALS_ID = 'git-ssh-id'  // Replace with the ID of your Jenkins Git credentials
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the repository to a workspace
                    git credentialsId: env.CREDENTIALS_ID, url: env.GIT_REPO_URL, branch: env.GIT_BRANCH
                }
            }
        }
        
        stage('Update File') {
            steps {
                script {
                    // Update the file content (e.g., appending a line)
                        sh "git config user.email gevi.reddy07@gmail.com"
                        sh "git config user.name GevireddyOfficial"
                        sh "cat deployment.yaml"
                        sh "sed -i 's+gevireddyofficial/smarthotelapp.*+gevireddyofficial/smarthotelapp:${DOCKERTAG}+g' deployment.yaml"
                        sh "cat deployment.yaml"
                }
            }
        }
        
        stage('Commit and Push') {
            steps {
                script {
                    // Commit the changes

                    sh "git add ."
                    sh "git commit -m 'Jenkins pipeline: Update file'"
                    
                    // Push the changes
                    withCredentials([sshUserPrivateKey(credentialsId: env.CREDENTIALS_ID, keyFileVariable: 'SSH_KEY')]) {
                        sh "GIT_SSH_COMMAND='ssh -o StrictHostKeyChecking=no -i $SSH_KEY' git push ${env.GIT_REPO_URL} ${env.GIT_BRANCH}"
                    }
                }
            }
        }
    }
}
