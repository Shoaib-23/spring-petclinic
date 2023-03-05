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
}
