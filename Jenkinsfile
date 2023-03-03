node('PHPINFO') {
    stage('VCS') {
        git url: 'https://github.com/Shoaib-23/spring-petclinic.git',
            branch: 'scripted'    
    }
    stage('Install') {
        sh './mvnw package'
    }
}
