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
                 } */
    stage("Publish to Nexus Repository Manager") {
            steps {
           nexusArtifactUploader credentialsId: 'nexus-server', groupId: 'Jenkins-user', nexusUrl: 'http://65.0.21.254:8081/repository/repo-with-tomcat-server-SNAP/', nexusVersion: 'nexus3', protocol: 'http', repository: 'repo-with-tomcat-server-SNAP', version: '3.43.0-01'  
                }
            }
        stage('deploy to tomcat server'){
            steps{
             deploy adapters: [tomcat8(credentialsId: '4d930877-e6d8-4b44-a69c-d731ed5923b2', path: '', url: 'http://ec2-52-198-120-232.ap-northeast-1.compute.amazonaws.com:8089/')], contextPath: null, war: '**/*.war'
            }
        } 
    }
}
