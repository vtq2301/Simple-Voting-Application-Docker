pipeline {
    agent any

    tools {}

    environment {
        dockerRegistry = "quentindev97/practice"
        registryCredential = "dockercreds"

    }

    stages {
        stage("GIT checkout") {
            steps{
                git branch: main, url: "https://github.com/vtq2301/Simple-Voting-Application-Docker.git"
            }
        }

        stage("Build Images") {
            steps {
                script {
                    resultImage = docker.build(dockerRegistry + ":$BUILD_NUMBER", "./result")
                    voteImage = docker.build(dockerRegistry + ":$BUILD_NUMBER", "./vote")
                    workerImage = docker.build(dockerRegistry + ":$BUILD_NUMBER", "./worker")
                }
            }
        }

        stage("Test") {
            steps {
                sh "echo 'Running Test...'"
            }
        }

        stage("Push images") {
            docker.withRegistry(dockerRegistry, registryCredentials) {
                resultImage.push("$BUILD_NUMBER")
                resultImage.push("latest")
            }
        }
    }

    post {

    }
}