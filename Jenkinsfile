pipeline{
    agent any 
    tools{
        maven 'Maven-Home'
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
        stage('SonarQube Analysis'){
            steps{
             withSonarQubeEnv('sonarqube-8.9'){
             sh "mvn sonar:sonar"
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
