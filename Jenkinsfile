pipeline{
    agent any
    options {
        timeout(time:1,units:'hour')
    }
    triggers{
        pollSCM('* * * * *')
    }
    stages{
        stage('sorce code'){
            steps{
                git url:'https://github.com/Prabhu028/springpetclinic_jfrog.git',
                    branch:'main'
            }
        }
        stage('build and package') {
            steps {
                 rtMavenDeployer (
                    id: "SPC_DEPLOYER",
                    serverId: "JFROG_CLOUD",
                    releaseRepo: 'vk-libs-snapshot-local',
                    snapshotRepo: 'vk-libs-snapshot-local'
                )
                rtMavenRun (
                    tool: 'MAVEN_3.9', 
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "SPC_DEPLOYER"
                    
                )
                rtPublishBuildInfo (
                    serverId: "JFROG_CLOUD"
                )
            }
        }
        stage('reporting') {
            steps {
                junit testResults: '**/target/surefire-reports/TEST-*.xml'
            }
    }
    }
}

