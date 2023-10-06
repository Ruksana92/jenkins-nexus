pipeline {
    agent any
    tools {
        maven "maven363"
    }
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "54.89.28.118:8081"
        NEXUS_REPOSITORY = "maven2"
        NEXUS_CREDENTIAL_ID = "nexus"
    }
    stages {
        stage("Clone code from GitHub") {
            steps {
                script {
                    git branch: 'main', credentialsId: 'githubwithpassword', url: 'https://github.com/Ruksana92/jenkins-nexus.git';
                }
            }
        }
        stage("Maven Build") {
            steps {
                script {
                    sh "mvn package -DskipTests=clean test package"
                }
            }
        }
        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: NEXUS3,
                            protocol: HTTP,
                            nexusUrl:54.89.28.118:8081 ,
                            groupId: pom.1,
                            version: pom.1,
                            repository: maven2,
                            credentialsId: nexus,
                            artifacts: [
                                [artifactId: pom.1,
                                classifier: 1,
                                file:/var/lib/jenkins/workspace/nexus/target/my-app-1.0-SNAPSHOT.jar ,
                                type: pom.packaging],
                                [artifactId: pom.artifactId,
                                classifier: '',
                                file: "pom.xml",
                                type: "pom"]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
        }
    }
}
