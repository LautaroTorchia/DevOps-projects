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
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "192.168.56.25:8081"
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
        /* stage("Sonar Scanner Analysis"){
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
                    -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                    -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
             }
        }  */
        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version} ARTVERSION";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: NEXUS_GRP_REPO,
                            version: ARTVERSION,
                            repository: RELEASE_REPO,
                            credentialsId: NEXUS_CREDENTIAL_ID,
                            artifacts: [
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: artifactPath,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } 
		    else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }


    }
}