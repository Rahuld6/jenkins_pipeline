pipeline {
    agent any

    environment {
        TOMCAT_IP = '172.31.14.230'               // Private IP of Tomcat server
        TOMCAT_USER = 'tomcat'                    // Tomcat Manager user
        TOMCAT_PASS = credentials('tomcat-manager') // Jenkins credential ID for password
        APP_NAME = 'myapp'                        // Name of your app
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the repo with the Jenkinsfile and project code
                git branch: 'main', url: 'https://github.com/Rahuld6/jenkins_pipeline.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                sshagent(['tomcat-ssh']) {
                    // Upload WAR to Tomcat server
                    sh """
                        scp -o StrictHostKeyChecking=no target/${APP_NAME}.war ec2-user@${TOMCAT_IP}:/tmp/${APP_NAME}.war
                    """

                    // Deploy via Tomcat Manager App text interface
                    sh """
                        ssh -o StrictHostKeyChecking=no ec2-user@${TOMCAT_IP} \\
                        "curl --upload-file /tmp/${APP_NAME}.war \\
                        --user ${TOMCAT_USER}:${TOMCAT_PASS} \\
                        http://localhost:8080/manager/text/deploy?path=/${APP_NAME}&update=true"
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully! Access your app at http://${TOMCAT_IP}:8080/${APP_NAME}"
        }
        failure {
            echo "Deployment failed. Check build and Tomcat logs."
        }
    }
}

