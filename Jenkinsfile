pipeline {
    agent { label 'PHPINFO' }
    stages {
        stage('git clone') {
            steps {
                git url: 'https://github.com/Shoaib-23/spring-petclinic.git',
                    branch: 'declarative'
            }
        }   
        stage('build') {
            steps {
                sh './mvnw package'
            }
        }
    }
}
