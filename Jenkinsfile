pipeline{
    agent any 
    tools{
        maven 'Maven-Home'
    }
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "65.0.21.254:8081"
        NEXUS_REPOSITORY = "repo-with-tomcat-server-SNAP"
        NEXUS_CREDENTIAL_ID = "nexus-server"
    }
    stages{
        
        stage('build'){
            steps{
                sh 'mvn clean package'
            }
            post{
                success{
                    echo "archiving the artifacts"
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        /*stage('SonarQube Analysis'){
           steps{
             withSonarQubeEnv('sonarqube-9.7.1.62043'){
             sh "mvn sonar:sonar"
        }
            }
              */   }
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
                            nexusVersion: NEXUS_VERSION,
                            protocol: NEXUS_PROTOCOL,
                            nexusUrl: NEXUS_URL,
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: NEXUS_REPOSITORY,
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
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
            }
       }
       
        stage('deploy to tomcat server'){
            steps{
             deploy adapters: [tomcat8(credentialsId: '4d930877-e6d8-4b44-a69c-d731ed5923b2', path: '', url: 'http://ec2-52-198-120-232.ap-northeast-1.compute.amazonaws.com:8089/')], contextPath: null, war: '**/*.war'
            }
        } 
    }
}
