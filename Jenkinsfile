pipeline {
    agent { label 'DOCKER' }    
    environment {
        APP_NAME = 'spring-petclinic'
        APP_JAR = "${APP_NAME}.jar"
        APP_PORT = '8080'
        APP_HOME = "/opt/${APP_NAME}"
        REMOTE_HOST = '15.206.82.251' // replace with actual remote host IP address or hostname
        REMOTE_USER = 'ubuntu' // replace with actual remote username
    }
    stages {
        stage('git clone') {
            steps {
                git url: 'https://github.com/Shoaib-23/spring-petclinic.git',
                    branch: 'release'
            }
        }
        stage('Build') {
            steps {
                sh './mvnw clean package' 
            }
        }   
        stage('Copy to Remote') {
            steps {
                sshagent(['vpc.pem']) { // replace with actual SSH key ID or path
                    sh "ssh -i ~/vpc.pem ${REMOTE_USER}@${REMOTE_HOST} mkdir -p ${APP_HOME}"
                    sh "scp target/${APP_JAR} ${REMOTE_USER}@${REMOTE_HOST}:${APP_HOME}/${APP_JAR}"
                }
            }
        }
        stage('Start Application') {
            steps {
                sshagent(['your-ssh-key']) {
                    sh "ssh ${REMOTE_USER}@${REMOTE_HOST} nohup java -jar ${APP_HOME}/${APP_JAR} --server.port=${APP_PORT} > ${APP_HOME}/${APP_NAME}.log 2>&1 &"
                }
            }
        }
    }
}
