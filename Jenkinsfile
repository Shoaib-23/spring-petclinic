pipeline {
    agent { label 'JENKINS-NODE' }
    triggers { pollSCM ('* * * * *') }
    parameters {
        choice(name: 'MAVEN_GOAL', choices: ['package', 'install', 'clean'], description: 'Maven Goal') }
    stages {
        stage('git clone') {
            steps {
                git url: 'https://github.com/Shoaib-23/spring-petclinic.git',
                    branch: 'declarative'
            }        
        }   
        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: 'https://shoaib23.jfrog.io/artifactory',
                    credentialsId: 'JFROG'
                )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: 'libs-release',
                    snapshotRepo: 'libs-snapshot'
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: 'libs-release',
                    snapshotRepo: 'libs-snapshot'
                )
            }
        }
        stage('package') {
            tools {
                jdk 'JDK-17'
            }
            steps {
                rtMavenRun (
                    tool: 'MAVEN',
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER"
                    
                )
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
                //sh "mvn ${params.MAVEN_GOAL}"
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
            mail subject: "Every stage in the pipeline is running successfully",
                body: "Use this URL ${BUILD_URL} for more info",
                to: 'shoaib.shoaib23@gmail.com',
                from: 'devops@qt.com'
        }
        failure {
            mail subject: "Some of the stage in the pipeline is failing",
                body: "Use this URL ${BUILD_URL} for more info",
                to: 'shoaib.shoaib23@gmail.com',
                from: 'devops@qt.com'
        }
    }
}