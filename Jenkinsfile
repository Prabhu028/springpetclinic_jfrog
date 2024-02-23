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
                git url:'',
                    branch:''
            }
        }
        stage('build and package') {
            steps {
                 rtMavenDeployer (
                    id: "SPC_DEPLOYER",
                    serverId: "JFROG_CLOUD",
                    releaseRepo: 'qt-app-libs-snapshot-local',
                    snapshotRepo: 'qt-app-libs-snapshot-local'
                )
                rtMavenRun (
                    tool: 'MAVEN_3.9', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "SPC_DEPLOYER"
                    //,
                    //buildName: "${JOB_NAME}",
                    //buildNumber: "${BUILD_ID}"
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

