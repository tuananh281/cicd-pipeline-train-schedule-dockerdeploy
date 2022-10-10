pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }

        stage('Build docker images') {
            steps {
                script {
                    app = docker.build("tuannanhh/train-schedule")
                    app.inside {
                        sh 'echo $(curl localhost:8080)'
                    }
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script: {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILDNUMBER}")
                        app.push("latest")
                    }
                }
            }
        }

        stage('Deploy Docker To Development') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariables: "USERPASS")]) {
                    script {
                        sh "sshpass -p '$USERNAME' ssh -o 'StrictHostKeyChecking=no' $USERNAME@dev_ip \"docker pull tuannanhh/train-schedule:${env.BUILDNUMBER}\""
                        try {
                            sh "sshpass -p '$USERNAME' ssh -o 'StrictHostKeyChecking=no' $USERNAME@dev_ip \"docker stop train-schedule\""
                            sh "sshpass -p '$USERNAME' ssh -o 'StrictHostKeyChecking=no' $USERNAME@dev_ip \"docker rm train-schedule\""
                        } catch (err) {
                            echo "caucht error: $err"
                        }
                        sh "sshpass -p '$USERNAME' ssh -o 'StrictHostKeyChecking=no' $USERNAME@dev_ip \"docker run --restart always train-schedule -p 8080:8080 -d tuannanhh/train-schedule:${env.BUILDNUMBER}\""
                    }
                }
            }
        }
    }
}