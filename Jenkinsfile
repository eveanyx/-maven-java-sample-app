pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Executing build'
                sh 'mvn compile'
            }
        }
        stage('Test'){
            steps{
                sh 'mvn test'
                
            }
        }
        stage('Package') {
            steps{
                sh 'mvn package'
            }
            post {
                always {
                // One or more steps need to be included within each condition's block.
                junit 'target/surefire-reports/*.xml'
            }
            success {
                // One or more steps need to be included within each condition's block.
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
                }
            }
        }
        stage('Deploy'){
            steps{
                withEnv(['JENKINS_NODE_COOKIE=dontKillMe']){
                    sh 'nohup java -jar -Dserver.port=8001 target/spring-petclinic-2.3.1.BUILD-SNAPSHOT.jar &'
                }
            }
        }
    }
}

