pipeline {

    agent any

    tools {
        maven "MAVEN3"
    }

    environment {
        SNAP_REPO="vprofile-snapshot"
        NEXUS_USER="admin"
        NEXUS_PASS="aaaaaa"
        RELEASE_REPO="vprofile-project-release"
        CENTRAL_REPO="vprofile-maven-dependencies"
        NEXUSIP="192.168.56.25"
        NEXUSPORT="8081"
        NEXUS_GRP_REPO="vprofile-group"
        NEXUS_LOGIN="nexuslogin"
    }
	
    stages{
        stage("Build"){
            steps{
                sh "mvn -s settings.xml -DskipTests install"
            }
            post{
                success{
                    echo "now Archiving"
                archiveArtifacts artifacts: '**/target/*.war', fingerprint: true 
                }
            }
        }
        stage("Test"){
            steps{
                sh "mvn test"
            }
        }
        stage("Checkstyle Analysis"){
            steps{
                sh "mvn checkstyle:checkstyle"
            }
        }

    }


}
