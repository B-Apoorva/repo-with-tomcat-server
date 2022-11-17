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
                    arciveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('deploy to tomcat server'){
            steps{
                deploy adapters: [tomcat8(credentialsId: 'e92fd400-79fe-421f-89fc-8077dc7d6c87', path: '', url: 'http://ec2-43-206-158-186.ap-northeast-1.compute.amazonaws.com:8089/')], contextPath: null, war: '**/*.war'
            }
        }
    }
}
