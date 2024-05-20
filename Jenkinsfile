pipeline {

    agent {
        label 'lab-server'
    }

    tools { 
        maven 'my-maven' 
    }

    stages {

        stage('Build with Maven') {
            steps {
                sh 'mvn --version'
                sh 'java -version'
                sh 'mvn clean package -Dmaven.test.failure.ignore=true'
            }
        }

        stage('Packaging/Pushing image') {

            steps {
                withDockerRegistry(credentialsId: 'docker-hub', url: 'https://index.docker.io/v1/') {
                    sh 'docker build -t mathoanghai92/shoeshop .'
                    sh 'docker push mathoanghai92/shoeshop'
                }
            }
        }

        stage('Deploy Shoe shop to lab-server') {
            steps {
                echo 'Deploying and cleaning'
                sh 'docker image pull mathoanghai92/shoeshop'
                sh 'docker container stop mathoanghai92-shoeshop || echo "this container does not exist" '
                sh 'docker network create dev || echo "this network exists"'
                sh 'echo y | docker container prune '

                sh 'docker container run -d --rm --name mathoanghai92-shoeshop -p 80:8080 --network dev mathoanghai92/shoeshop'
            }
        }
 
    }
    post {
        // Clean after build
        always {
            cleanWs()
        }
    }
}
