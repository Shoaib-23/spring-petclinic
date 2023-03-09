pipeline {
    agent { label 'JENKINS-NODE' }
    triggers { pollSCM ('* * * * *') }
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean'], description: 'Maven Goal') }
    stages {
        stage('git clone') {
            steps {
                post {
                    always {
                        mail subject: "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is start cloning",
                             body: "Use this URL ${BUILD_URL} for more info",
                             to: 'shoaib.shoaib23@gmail.com',
                             from: 'devops@qt.com'
                    }
                }
                git url: 'https://github.com/Shoaib-23/spring-petclinic.git',
                    branch: 'declarative'
            }
        }   
        stage('build') {
            tools {
                jdk 'JDK_17_UBUNTU'
            }
            steps {
                sh "./mvnw ${params.MAVEN_GOAL}"
            }
        }
        stage('sonar analysis') {
            steps {
                withSonarQubeEnv('SONAR_CLOUD') { // Will pick the global server connection you have configured
                    sh './mvnw verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=spring-petclinic23_spring -Dsonar.organization=spring-petclinic23'
                }
            }
        }
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
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
