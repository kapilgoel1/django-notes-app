@Library('my-shared-library')_
pipeline {
    agent {label "vinod"}

    stages {
        stage('Code') {
            steps {
                echo 'This is cloning the code'
                git url: 'https://github.com/kapilgoel1/django-notes-app', branch: 'main'
                echo 'Code Clone is sucessful!'
            }
        }
        stage('Build') {
            steps {
                echo 'This is building the code'
                script{
                docker_build("notes-app", "latest")
                }
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials(
                    [usernamePassword(
                        credentialsId:"dockerHubCred",
                        passwordVariable:"dockerHubPass", 
                        usernameVariable:"dockerHubUser"
                        )
                    ]
                ){
                sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage('Test') {
            steps {
                echo 'This is testing the code'
            }
        }
        stage('Deploy') {
            steps {
                echo 'This is deploying the code'
                sh 'docker run -d -p 8000:8000 notes-app:latest'
            }
        }
    }
}
