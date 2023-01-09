node 
{
        // stage('Cloning the project from Git') {
        //     checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'git_repo', url: 'https://github.com/tuananh281/cicd-pipeline-train-schedule-dockerdeploy.git']])
        // }
    stage('Sonarqube Analysis') {
        def scannerHome = tool 'sonarqube';
            withSonarQubeEnv('sonarqube_token') {
            sh """/var/lib/jenkins/tools/hudson.plugins.sonar.SornarRunnerInstallation/sonarqube/bin/sonar-scanner \
            -D sonar.projectVersion=1.0-SNAPSHOT \
                -D sonar.login=admin \
                -D sonar.password=28112002 \
                -D sonar.projectBaseDir=/var/lib/jenkins/workspace/train-schedule/ \
                -D sonar.projectKey=test \
                -D sonar.language=js \
                -D sonar.sourceEncoding=UTF-8 \
                -D sonar.sources=test/src/main \
                -D sonar.tests=test/src/test \
                -D sonar.host.url=http://172.16.94.15:9000/"""
            }
    }

}


// properties([disableConcurrentBuilds()])
// pipeline {
//   agent any
//   tools {
//     nodejs 'test_nodejs'
//   }
//   options {
//     // This is required if you want to clean before build
//     skipDefaultCheckout(true)
//   }
//   stages {
//     stage('Clone code SCM for sonar') {
//       steps {
//         // Clean before build
//         cleanWs()
//         git branch: 'main',
//           credentialsId: 'tuananh_github',
//           url: 'git@github.com:tuananh281/cicd-pipeline-train-schedule-dockerdeploy.git'
//       }
//     }
//     stage('SonarQube analysis') {
//       steps {
//         script {
//           def scannerHome = tool 'SonarQube Scanner';
//           withSonarQubeEnv('SonarQube Server') {
//             sh "${tool("sonarscan")}/bin/sonar-scanner -Dsonar.projectKey=reactapp -Dsonar.projectName=reactapp"
//           }
//         }
//       }
//     }
//     stage("Quality gate") {
//       steps {
//         script {
//           def qualitygate = waitForQualityGate()
//           sleep(10)
//           if (qualitygate.status != "OK") {
//             waitForQualityGate abortPipeline: true
//           }
//         }
//       }
//     }
//   }
// }


// pipeline {
//     agent any
//     stages {
//         // stage('Build') {
//         //     steps {
//         //         echo 'Running build automation'
//         //         sh './gradlew build --no-daemon'
//         //         archiveArtifacts artifacts: 'dist/trainSchedule.zip'
//         //     }
//         // }
//         // stage('Checkout Source') {
//         //     steps {
//         //          git 'https://github.com/tuananh281/cicd-pipeline-train-schedule-dockerdeploy.git'
//         //     }
//         // }
//         tools {nodejs “test_nodejs”}
//         stage('Test code') {
//             steps {
//                 sh "npm config ls"
//             }
//         }

//         stage('Build docker images') {
//             steps {
//                 script {
//                     DOCKER_IMAGE="test-gitops"
//                     app = docker.build("tuannanhh/${DOCKER_IMAGE}")
//                     // app.inside {
//                     //     sh 'echo $(curl localhost:8080)'
//                     // }
//                 }
//             }
//         }

//         stage('Push Docker Image') {
//             steps {
//                 script {
//                     DOCKER_REGISTRY="registry.hub.docker.com"
//                     DOCKER_NAME="tuannanhh"

//                     docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_tuananh') {
//                         app.push("${env.BUILD_NUMBER}")
//                         // app.push("latest")
//                     }
//                     sh "docker image rm ${DOCKER_NAME}/${DOCKER_IMAGE}:latest"
//                     sh "docker image rm ${DOCKER_REGISTRY}/${DOCKER_NAME}/${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
//                 }
//             }
//         }

//         stage('Trigger ManifestUpdate') {
//             steps {
//                 echo "Update manifestjob"
//                 build job: 'update-manifest-github', 
//                     parameters: [
//                         string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)
//                     ]   
//             }
//         }
    
//     }
// }




//         stage('Deploy k8s') {
//             steps{
//                  withCredentials([string(credentialsId: "argocd_role", variable: 'ARGOCD_AUTH_TOKEN')]) {
//                      sh '''
//                      ARGOCD_SERVER="argocd-deploy.com"
//                      APP_NAME=tuananh-dep-k8s
                     
//                      ARGOCD_SERVER=$ARGOCD_SERVER argocd --grpc-web app sync $APP_NAME --force
//                      ARGOCD_SERVER=$ARGOCD_SERVER argocd --grpc-web app wait $APP_NAME --timeout 600
//                      '''
//                 }
//             }
//         }    
        
        // stage('Test') {
        //     steps {
        //         echo 'Testing...'
        //         sh 'npm test'
        //     }
        // }

//         stage('Deploying App to Kubernetes') {
//             steps {
//                 // input 'Do you want to deploy to K8S Cluster ?'
//                 // milestone(1)
//                 script {
//                     kubernetesDeploy(configs: 'deploy/deployment.yml', kubeconfigId: 'k8s_tuananh')
//                 }
//             }
//         }

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
