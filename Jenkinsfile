node {
    def dockerImage    
    def dockerRepoUrl = "localhost:8083"
    def dockerImageName = "hello-world-java"
    def dockerImageTag = "${dockerRepoUrl}/${dockerImageName}:${env.BUILD_NUMBER}"
    def TASK_FAMILY = "ecs-fargate-cluster-svc1"
    
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
}
