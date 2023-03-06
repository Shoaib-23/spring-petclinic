pipeline {
    agent { label 'JENKINS-NODE' }
    stages {
        stage('git clone') {
            steps {
                git url: 'https://github.com/Shoaib-23/spring-petclinic.git',
                    branch: 'declarative'
            }
        }   
        stage('build') {
            steps {
                sh './gradlew build'
            }
        }
    }
    post {
        failure {
            emailext body: "The build failed. Please check the logs for details.",
                     subject: "Build Failed: ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}",
                     to: "shoaib.shoaib23@gmail.com"
        }
        success {
            emailext body: "The build succeeded. Please check the artifact for details.",
                     subject: "Build Succeeded: ${env.JOB_NAME} - Build #${env.BUILD_NUMBER}",
                     to: "shoaib.shoaib23@gmail.com"
        }
    }
}
