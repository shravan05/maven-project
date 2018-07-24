pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
	stage ('Deploy to Dev'){
            steps {
                build job: 'deploy-to-dev'
            }
        }
	stage ('Deploy to QA'){
            steps{
                timeout(time:5, unit:'DAYS'){
                input message:'Approve QA Deployment?'
                }
		
		build job: 'deploy-to-qa'
            }
            post {
                success {
                    echo 'Code deployed to QA ENV.'
                }
            }
        }
    }
}
