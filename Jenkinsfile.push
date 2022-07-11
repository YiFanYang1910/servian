pipeline {
    agent any 

    environment {
        AWS_ACCOUNT_ID="219392032829"
        AWS_DEFAULT_REGION="ap-southeast-2" 
        CLUSTER_NAME="jamesservian_cluster"
        SERVICE_NAME="jamesservian_ecs_service"
        TASK_DEFINITION_NAME="jamesservian_task_definition"
        DESIRED_COUNT="2"
        IMAGE_REPO_NAME="jamesservian"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "219392032829.dkr.ecr.ap-southeast-2.amazonaws.com/jamesservian"
    }

    stages {
       
        stage('Building image') {
            steps {
                dir('./'){
                    sh 'docker build -t servian-test .'
                }             
            }
        }

        stage('Pushing to ECR') {
            steps {
                withAWS(credentials: 'AWS james Full'){
                dir('./'){
                    sh 'docker tag servian-test:latest 219392032829.dkr.ecr.ap-southeast-2.amazonaws.com/jamesservian/servian-test:latest'
                    sh 'docker push 219392032829.dkr.ecr.ap-southeast-2.amazonaws.com/jamesservian/servian-test'
                    }  
                }
            }
        }

        stage('Deploy') {
            steps{
                dir('.'){
                        script {
                            sh 'chmod +x -R /var/lib/jenkins/workspace/servian/script.sh'
                            sh './script.sh'
                            }
                    }     
                }
        }    

    }

}
