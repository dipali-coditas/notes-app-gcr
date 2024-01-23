pipeline {
    agent any
    
    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/dipali-coditas/notes-app-gcr.git", branch: "main"
            }
        }
        stage('Build Stage') {
            steps {
                echo 'Build Stage'
                script {
                    ver = readFile('version').trim()
                    sh "docker build -t ${ver} ."
                }
            }
        }
        stage('Artifact Push Stage') {
            steps {
                echo 'Artifact Push Stage'
                script {
                    withCredentials([file(credentialsId: 'service_acc', variable: 'service_acc')]) {
                        sh "docker login -u _json_key --password-stdin https://asia-south1-docker.pkg.dev < \$service_acc"
                        sh "docker push ${ver}"
                    }
                }
            }
        }
    }
}
