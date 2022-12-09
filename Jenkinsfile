pipeline {

    agent any

    tools {
        maven "MAVEN3"
       /* jdk "JDK8" */
    }

    environment {
        SNAP_REPO="vprofile-snapshot"
        NEXUS_USER="admin"
        NEXUS_PASS="pin123"
        RELEASE_REPO="vprofile-project-release"
        CENTRAL_REPO="vprofile-maven-dependencies"
        NEXUSIP="192.168.56.25"
        NEXUSPORT="8081"
        NEXUS-GRP-REPO="vprofile-group"
        NEXUS_LOGIN="nexuslogin"
    }
	
    stages{
        stage("Build"){
            steps{
                sh "mvn -s settings.xml -DskipTests install"
            }
        }

    }


}
