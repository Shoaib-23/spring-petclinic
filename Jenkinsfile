Pipeline {
    Agent { label 'PHPINFO' }
    Stages {
        Stage('git clone') {
            steps {
            git url: 'https://github.com/Shoaib-23/spring-petclinic.git',
                branch: declarative
            }
        }   
        stage('build') {
            steps {
                sh './mvnw package'
            }
        }
    }
}
