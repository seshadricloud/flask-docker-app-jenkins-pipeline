pipeline {
    agent any
    environment {
        DOCKER_HUB_REPO = "talha1995/test"
        CONTAINER_NAME = "flask-container"
        STUB_VALUE = "200"
    }
    stages {
        stage('Stubs-Replacement'){
            steps {
                // 'STUB_VALUE' Environment Variable declared in Jenkins Configuration 
                echo "STUB_VALUE = ${STUB_VALUE}"
                sh "sed -i 's/<STUB_VALUE>/$STUB_VALUE/g' config.py"
                sh 'cat config.py'
            }
        }
        stage('Build') {
            steps {
                //  Building new image
                sh 'sudo docker image build -t $DOCKER_HUB_REPO:latest .'
                sh 'sudo docker image tag $DOCKER_HUB_REPO:latest $DOCKER_HUB_REPO:$BUILD_NUMBER'

                // //  Pushing Image to Repository
                // sh 'sudo docker push talha1995/test:$BUILD_NUMBER'
                // sh 'sudo docker push talha1995/test:latest'
                
                echo "Image built and pushed to repository"
            }
        }
//         stage('Deploy') {
//             steps {
//                 script{
//                     //sh 'BUILD_NUMBER = ${BUILD_NUMBER}'
//                     if (BUILD_NUMBER == "1") {
//                         sh 'sudo docker run --name $CONTAINER_NAME -d -p 5000:5000 $DOCKER_HUB_REPO'
//                     }
//                     else {
//                         sh 'sudo docker stop $CONTAINER_NAME'
//                         sh 'sudo docker rm $CONTAINER_NAME'
//                         sh 'sudo docker run --name $CONTAINER_NAME -d -p 5000:5000 $DOCKER_HUB_REPO'
//                     }
//                     //sh 'echo "Latest image/code deployed"'
//                 }
//             }
//         }
//     }
// }
stage('Deploy') {
    steps {
        script {
            if (BUILD_NUMBER == "1" || !sh(returnStdout: true, script: "sudo docker ps -a | grep $CONTAINER_NAME").trim()) {
                sh "sudo docker run --name $CONTAINER_NAME -d -p 5000:5000 $DOCKER_HUB_REPO"
            } else {
                sh "sudo docker stop $CONTAINER_NAME"
                sh "sudo docker rm $CONTAINER_NAME"
                sh "sudo docker run --name $CONTAINER_NAME -d -p 5000:5000 $DOCKER_HUB_REPO"
            }
        }
    }
}
