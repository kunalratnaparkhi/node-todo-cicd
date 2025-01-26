pipeline {
    agent any
    
    stages {
        stage("CodeClone") {
            steps {
                echo "Code Clone"
                git 'https://github.com/kunalratnaparkhi/node-todo-cicd.git'
            }
        }
        
        stage("Build") {
            steps {
                echo "Build"
                sh "docker build -t my-note-app ."
            }
        }
        
        stage("MoveToDockerHub") {
            steps {
                echo "Move image to DockerHub"
                withCredentials([usernamePassword(credentialsId: 'DockerHubID', passwordVariable: 'dockerPass', usernameVariable: 'dockerID')]) {
                    sh "docker tag my-note-app ${env.dockerID}/my-note-app:${BUILD_NUMBER}"
                    sh "docker login -u ${env.dockerID} -p ${env.dockerPass}"
                    sh "docker push ${env.dockerID}/my-note-app:${build_number}"
                }
            }
        }
        
        stage("DeployContainer") {
            steps {
                echo "Deploy Container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
