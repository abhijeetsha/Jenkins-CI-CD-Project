pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/abhijeetsha/Jenkins-CI-CD-Project.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$BUILD_NUMBER .'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([string(credentialsId: 'docker-pass', variable: 'DOCKER_PASS')]) {
                    sh '''
                    docker login -u yourdockerhub -p $DOCKER_PASS
                    docker push $DOCKER_IMAGE:$BUILD_NUMBER
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f app || true
                docker run -d -p 8000:8000 --name app $DOCKER_IMAGE:$BUILD_NUMBER
                '''
            }
        }
    }
}

