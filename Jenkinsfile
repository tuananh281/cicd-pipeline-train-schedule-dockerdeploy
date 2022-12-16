pipeline {
    agent any
    stages {
        // stage('Build') {
        //     steps {
        //         echo 'Running build automation'
        //         sh './gradlew build --no-daemon'
        //         archiveArtifacts artifacts: 'dist/trainSchedule.zip'
        //     }
        // }
        // stage('Checkout Source') {
        //     steps {
        //          git 'https://github.com/tuananh281/cicd-pipeline-train-schedule-dockerdeploy.git'
        //     }
        // }

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
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_tuananh') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
                sh 'npm test'
            }
        }

        stage('Deploying App to Kubernetes') {
            steps {
                input 'Do you want to deploy to K8S Cluster ?'
                milestone(1)
                script {
                    kubernetesDeploy(configs: 'deployment.yml', kubeconfigId: 'k8s_tuananh')
                }
            }
        }

//         stage('Deploy Docker To Development') {
//             steps {
//                 withCredentials([usernamePassword(credentialsId: 'webserver-deploy', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
//                     script {
//                         sh "sshpass -p '$USERPASS' ssh -o 'StrictHostKeyChecking=no' $USERNAME@$dev_ip \' docker pull tuannanhh/train-schedule:${env.BUILD_NUMBER}\'"
//                         try {
//                             sh "sshpass -p '$USERPASS' ssh -o 'StrictHostKeyChecking=no' $USERNAME@$dev_ip \' docker stop train-schedule\'"
//                             sh "sshpass -p '$USERPASS' ssh -o 'StrictHostKeyChecking=no' $USERNAME@$dev_ip \' docker rm train-schedule\'"
//                         } catch (err) {
//                             echo 'caucht error: $err'
//                         }
//                         sh "sshpass -p '$USERPASS' ssh -o 'StrictHostKeyChecking=no' $USERNAME@$dev_ip \' docker run --restart always --name train-schedule -p 8080:8080 -d tuannanhh/train-schedule:${env.BUILD_NUMBER}\'"
//                     }
//                 }
//             }
//         }

//         stage('Deploy Docker To Stagging') {
//             steps {
//                 withCredentials([usernamePassword(credentialsId: 'webserver-deploy', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
//                     script {
//                         sh "sshpass -p '$USERPASS' ssh -o 'StrictHostKeyChecking=no' $USERNAME@$staging_ip \' docker pull tuannanhh/train-schedule:${env.BUILD_NUMBER}\'"
//                         try {
//                             sh "sshpass -p '$USERPASS' ssh -o 'StrictHostKeyChecking=no' $USERNAME@$staging_ip \' docker stop train-schedule\'"
//                             sh "sshpass -p '$USERPASS' ssh -o 'StrictHostKeyChecking=no' $USERNAME@$staging_ip \' docker rm train-schedule\'"
//                         } catch (err) {
//                             echo 'caucht error: $err'
//                         }
//                         sh "sshpass -p '$USERPASS' ssh -o 'StrictHostKeyChecking=no' $USERNAME@$staging_ip \' docker run --restart always --name train-schedule -p 8080:8080 -d tuannanhh/train-schedule:${env.BUILD_NUMBER}\'"
//                     }
//                 }
//             }
//         }

//         stage('Deploy Docker To Production') {
//             steps {
//                 input 'Do you want to deploy to production ?'
//                 milestone(1)
//                 withCredentials([usernamePassword(credentialsId: 'webserver-deploy', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
//                     script {
//                         sh "sshpass -p '$USERPASS' ssh -o 'StrictHostKeyChecking=no' $USERNAME@$prod_ip \' docker pull tuannanhh/train-schedule:${env.BUILD_NUMBER}\'"
//                         try {
//                             sh "sshpass -p '$USERPASS' ssh -o 'StrictHostKeyChecking=no' $USERNAME@$prod_ip \' docker stop train-schedule\'"
//                             sh "sshpass -p '$USERPASS' ssh -o 'StrictHostKeyChecking=no' $USERNAME@$prod_ip \' docker rm train-schedule\'"
//                         } catch (err) {
//                             echo 'caucht error: $err'
//                         }
//                         sh "sshpass -p '$USERPASS' ssh -o 'StrictHostKeyChecking=no' $USERNAME@$prod_ip\' docker run --restart always --name train-schedule -p 8080:8080 -d tuannanhh/train-schedule:${env.BUILD_NUMBER}\'"
//                     }
//                 }
//             }
//         }
    }
}