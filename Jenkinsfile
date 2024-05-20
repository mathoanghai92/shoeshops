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

    }
    post {
        // Clean after build
        always {
            cleanWs()
        }
    }
}
