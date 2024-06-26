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
                sh (script: "${buildScript}", label: "Build with Maven")
            }
        }

        stage('Deploy') {
            steps {
                script {
                    try {
                        timeout(time: 5, unit: 'MINUTES') {
                            env.useChoice = input message: "Can it be deployed?",
                                parameters: [choice(name: 'deploy', choices: 'no\nyes', description: 'Choose "yes" if you want to deploy')]
                        }
                        if (env.useChoice == 'yes') {
                            sh (script: "${copyScript}", label: "Copy the JAR file to the deployment folder")
                            sh (script: "${permsScript}", label: "Set permission for the deployment folder")
                            sh (script: "${killScript}", label: "Terminate the running process")
                            sh (script: "${runScript}", label: "Run the project")
                        } else {
                            echo "Deployment not confirmed. Doing nothing."
                        }
                    } catch (Exception err) {
                        echo "Caught exception: ${err}"
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
