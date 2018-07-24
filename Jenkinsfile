pipeline {
    agent none
    stages{
        stage('Build'){
            agent {
       	      label 'Node1'
     	    }
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
            agent {
              label 'Node1'
            }
	    steps {
                build job: 'deploy-to-dev'
            }
        }
	stage ('Deploy to QA'){
            agent {
              label 'Node1'
            }
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

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
	stage ('Deploy to PROD'){
            agent {
              label 'Node1'
            }
	    steps{
                timeout(time:5, unit:'DAYS'){
                input message:'Approve PROD Deployment?'
                }

                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code deployed to PROD ENV.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
    }
}
