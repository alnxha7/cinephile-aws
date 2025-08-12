pipeline {
    agent any

    environment {
        IMAGE_NAME = "cinephile"
        CONTAINER_NAME = "cinephile_container"
        DOCKER_PORT = "8000"
        HOST_PORT = "8000"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/alnxha7/cinephile-aws.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('cinephile/cinephile/cinephile_project') {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Stop Old Container') {
            steps {
                script {
                    sh '''
                        if [ "$(docker ps -q -f name=$CONTAINER_NAME)" ]; then
                            docker stop $CONTAINER_NAME
                            docker rm $CONTAINER_NAME
                        fi
                    '''
                }
            }
        }

        stage('Run New Container') {
            steps {
                sh 'docker run -d --name $CONTAINER_NAME -p $HOST_PORT:$DOCKER_PORT --restart unless-stopped $IMAGE_NAME'
            }
        }
    }
}
