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
        stage('deploy to tomcat server'){
            steps{
               deploy adapters: [tomcat8(credentialsId: 'b88ab99e-d96e-45a4-a6ee-4c1a36d8f949', path: '', url: 'http://ec2-43-206-158-186.ap-northeast-1.compute.amazonaws.com:8089/')], contextPath: null, war: '**/*.war'
            }
        }
    }
}
