pipeline {
    agent any 

    environment {
        AWS_ACCOUNT_ID="219392032829"
        AWS_DEFAULT_REGION="ap-southeast-2" 
        CLUSTER_NAME="default"
        SERVICE_NAME="bookinglettest-service"
        TASK_DEFINITION_NAME="first-run-task-definition"
        DESIRED_COUNT="2"
        IMAGE_REPO_NAME="219392032829.dkr.ecr.ap-southeast-2.amazonaws.com/bookinglet-backend-test"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
	    registryCredential = "demo-admin-user"
    }

    stages {
        stage('Pushing to ECR') {
            steps {
                dir('./'){
                    sh 'docker tag servian/techchallengeapp:latest 219392032829.dkr.ecr.ap-southeast-2.amazonaws.com/servian/techchallengeapp:latest'
                    sh 'docker push 219392032829.dkr.ecr.ap-southeast-2.amazonaws.com/servian/techchallengeapp:latest'
                }  
            }
        }

        stage('Deploy') {
            steps{
                dir('.'){
                    withAWS(credentials: registryCredential, region: "${AWS_DEFAULT_REGION}") {
                        script {
                            sh 'chmod +x -R /var/lib/jenkins/workspace/bookingletBackend/back_end/script.sh'
                            sh './script.sh'
                            }
                        }
                    }     
                }
        }    

    }

}
