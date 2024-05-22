pipeline {
    agent {
        label 'lab-server'
    }
    stages {
        stage('Info') {
            steps {
                sh("""whoami;pwd;ls -la""", label: "first stage" )
            }
        }
}
