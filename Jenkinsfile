pipeline {

    agent any

    tools {
        maven "MAVEN3"
       /* jdk "JDK8" */
    }

    environment {
        SNAP_REPO = "vprofile-snapshot"
        NEXUS_USER = "admin"
        NEXUS_PASS = "pin123"
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "192.168.56.25"
        NEXUS_PORT = "8081"
        NEXUS_REPOSITORY = "vprofile-project-release"
	    NEXUS_REPOGRP_ID    = "vprofile-group"
        NEXUS_CREDENTIAL_ID = "nexuslogin"
        ARTVERSION = "${env.BUILD_ID}"
    }
	
    stages{
        stage("Build"){
            steps{
                sh "mvn -s settings.xml -DskipTests install"
            }
        }

    }


}
