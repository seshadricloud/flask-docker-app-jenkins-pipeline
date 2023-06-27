pipeline {
    agent any
    environment {
        CONTAINER_NAME = "flask-container"
        STUB_VALUE = "200"
    }
    stages {
        stage('Stubs-Replacement') {
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
                    sh 'sudo docker rm -f $CONTAINER_NAME || true'
                    sh 'sudo docker run --name $CONTAINER_NAME -d -p 8000:5000 my-flask-app'
                }
            }
        }
    }
}
                        
