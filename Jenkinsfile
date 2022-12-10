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
        SONARSERVER="sonarserver"
        SONARSCANNER="sonarscanner"
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
        stage("Checkstyle Analysis"){
            steps{
                sh "mvn checkstyle:checkstyle"
            }
        }
        stage("Sonar Scanner Analysis"){
            environment{
                scannerHome= tool "${SONARSCANNER}"
            }
            steps{
                withSonarQubeEnv("${SONARSERVER}"){
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                    -Dsonar.projectName=DevOps-projects \
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.sources=src/ \
                    -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                    -Dsonar.junit.reportsPath=target/surefire-reports/ \
                    -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
             }
        }


    }
}