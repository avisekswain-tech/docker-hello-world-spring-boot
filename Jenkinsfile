  node {
    def taskfamily1 = "ecs-fargate-cluster-svc1"    
    def dockerRepoUrl = "localhost:8083"
    def dockerImageName = "hello-world-java"
    def dockerImageTag = "${dockerRepoUrl}/${dockerImageName}:${env.BUILD_NUMBER}"
    def taskfamily = "ecs-fargate-cluster-svc1"
	
    stage('Clone Repo') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/dstar55/docker-hello-world-spring-boot.git'
    }    
  
    stage('Build Project') {
	sh  "/usr/share/maven/bin/mvn package"
    }
		
    stage('Build Docker Image') {
      // build docker image
	sh "sudo docker build -t helloworldjavatest:$BUILD_NUMBER ."
    }
	
    stage('Push Docker Image to ECR')
    withAWS(role:'AdminAccess-IAM-Role', roleAccount:'474173922354')
	{
	sh "sudo docker tag helloworldjavatest:$BUILD_NUMBER 474173922354.dkr.ecr.us-east-1.amazonaws.com/testrepo:$BUILD_NUMBER"
	sh "sudo docker push 474173922354.dkr.ecr.us-east-1.amazonaws.com/testrepo:latest"
    }
	
    stage('Task Definition Creation')
    withAWS(role:'AdminAccess-IAM-Role', roleAccount:'474173922354')
	{
	echo "${taskfamily1}"
//	sh 'sed -e "s;%BUILD_NUMBER%;${BUILD_NUMBER};g" ${taskfamily}.json > ${taskfamily}-${BUILD_NUMBER}.json'
	sh 'aws ecs register-task-definition --family ecs-fargate-cluster-svc1 --cli-input-json file://ecs-fargate-cluster-svc1.json --region us-east-1'
        sh "aws ecs update-service --cluster ecs-fargate-cluster-test1 --service ecs-fargate-cluster-svc1 --task-definition ecs-fargate-cluster-svc1 --desired-count 1 --region us-east-1"
    }
}
