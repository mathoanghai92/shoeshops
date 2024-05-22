pipeline {
    agent {
        label 'lab-server'
    }
    environment { 
        appUser = "shoeshop"
        appName = "shoe-ShoppingCart"
        appVersion = "0.0.1-SNAPSHOT"
        appType = "jar"
        processName = "${appName}-${appVersion}.${appType}"
        folderDeploy = "/datas/${appUser}"
        buildScript = "mvn clean install -DskipTests-true"
    }

    stages {
        stage('Info') {
            steps {
                sh(script: """ whoami;pwd;ls -la """, label: "first stage" )
            }
        }
    stage('build') {

            steps {
                sh (script: """ ${buildScript} """, label: "build with maven")
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
