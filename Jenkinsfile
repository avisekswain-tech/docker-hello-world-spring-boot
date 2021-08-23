node {
    //def dockerImage    
    //def dockerRepoUrl = "localhost:8083"
    //def dockerImageName = "hello-world-java"
    //def dockerImageTag = "${dockerRepoUrl}/${dockerImageName}:${env.BUILD_NUMBER}"
    //def taskfamily = "ecs-fargate-cluster-svc1"
    
    environment {
        branch_name = "master"
        git_url = "https://github.com/pemmasani1200/DevOpsClassCodes.git"
        SERVICE_NAME = "ecs-fargate-cluster-svc"
        IMAGE_VERSION = "${BUILD_NUMBER}"
        taskfamily = "ecs-fargate-cluster-svc1"
        DESIRED_COUNT = "1"
        REGION = "us-east-1"
        CLUSTER_NAME = "ecs-fargate-cluster-test"
	//ECR_REPO = "011194234014.dkr.ecr.us-east-2.amazonaws.com/irving"
    }
	
    stage('Clone Repo') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/dstar55/docker-hello-world-spring-boot.git'
    }    
  
    stage('Build Project') {
	sh  "/usr/share/maven/bin/mvn package"
    }
		
    stage('Build Docker Image') {
      // build docker image
	sh "sudo docker build -t helloworldjava:$BUILD_NUMBER ."
    }
	
    stage('Push Docker Image to ECR')
    withAWS(role:'AdminAccess-IAM-Role', roleAccount:'474173922354')
	{
	sh "sudo docker tag helloworldjava:$BUILD_NUMBER 474173922354.dkr.ecr.us-east-1.amazonaws.com/tomcatapp:$BUILD_NUMBER"
	sh "sudo docker push 474173922354.dkr.ecr.us-east-1.amazonaws.com/tomcatapp:$BUILD_NUMBER"
    }
    stage('Task Definition Creation')
    withAWS(role:'AdminAccess-IAM-Role', roleAccount:'474173922354')
	{
//	sh 'sed -e "s;%BUILD_NUMBER%;${BUILD_NUMBER};g" ${taskfamily}.json > ${taskfamily}-${BUILD_NUMBER}.json'
	sh 'aws ecs register-task-definition --family ecs-fargate-cluster-svc1 --cli-input-json file://ecs-fargate-cluster-svc1.json'
    }
}
