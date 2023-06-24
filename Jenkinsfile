pipeline {
    agent any
    environment {
        CONTAINER_NAME = "flask-container"
        STUB_VALUE = "200"
    }
    stages {
        stage('Stubs-Replacement'){
            steps {
                echo "STUB_VALUE = ${STUB_VALUE}"
                sh "sed -i 's/<STUB_VALUE>/$STUB_VALUE/g' config.py"
                sh 'cat config.py'
            }
        }
        stage('Build') {
            steps {
                sh 'sudo docker image build -t my-flask-app:latest .'
                echo "Image built successfully"
            }
        }
        stage('Deploy') {
            steps {
                script {
                    if (BUILD_NUMBER == "1") {
                        sh 'sudo docker run --name $CONTAINER_NAME -d -p 5000:5000 my-flask-app'
                    } else {
                        sh 'sudo docker stop $CONTAINER_NAME'
                        sh 'sudo docker rm $CONTAINER_NAME'
                        sh 'sudo docker run --name $CONTAINER_NAME -d -p 5000:5000 my-flask-app'
                    }
                }
            }
        }
    }
}
