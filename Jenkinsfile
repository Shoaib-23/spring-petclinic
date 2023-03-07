pipeline {
    agent { label 'JENKINS-NODE' }
    triggers { pollSCM ('') }
    parameters {
        choice(name: 'GRADLE_GOAL', choices: ['build', 'install', 'clean'], description: 'Gradle Goal') }
    stages {
        stage('git clone') {
            steps {
                git url: 'https://github.com/Shoaib-23/spring-petclinic.git',
                    branch: 'declarative'
            }
        }   
        stage('build') {
            tools {
                jdk 'JDK_17_UBUNTU'
            }
            steps {
                sh "./gradlew ${params.GRADLE_GOAL}"
            }
        }
        stage('sonar analysis') {
            steps {
                withSonarQubeEnv() { // Will pick the global server connection you have configured
                    sh './gradlew sonarqube'
                }
            }
        }
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/build/libs/spring-petclinic-3.0.0.jar',
                                 onlyIfSuccessful: true
                junit testResults: '**/TEST-*.xml'
            }
        }
    }
    post {
        success {
            mail subject: "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is success",
                body: "Use this URL ${BUILD_URL} for more info",
                to: 'shoaib.shoaib23@gmail.com',
                from: 'devops@qt.com'
        }
        failure {
            mail subject: "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is failed",
                body: "Use this URL ${BUILD_URL} for more info",
                to: 'shoaib.shoaib23@gmail.com',
                from: 'devops@qt.com'
        }
    }
}
