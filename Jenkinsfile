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
        buildScript = "mvn clean install -DskipTests=true"
        copyScript = "sudo cp target/${processName} ${folderDeploy}"
        permsScript = "sudo chown -R ${appUser}. ${folderDeploy}"
        killScript = "sudo kill -9 \$(ps -ef | grep ${processName} | grep -v grep | awk '{print \$2}')"
        runScript = "sudo su ${appUser} -c \"cd ${folderDeploy}; java -jar ${processName} > nohup.out 2>&1 &\""
    }

    stages {

        stage('Build') {
            steps {
                sh (script: "${buildScript}", label: "build with maven")
            }
        }

        stage('Deploy') {
            steps {
                sh (script: "${copyScript}", label: "copy the file jar to folder deploy")
                sh (script: "${permsScript}", label: "set permission for folder deploy")
                sh (script: "${killScript}", label: "terminate running process!!!")
                sh (script: "${runScript}", label: "run the project")
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
