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
/*	  
    stage('Maven_Compile') {
            sh  "/opt/maven/bin/mvn compile"
    }
    stage('Maven_unittesting') {
            sh  "/opt/maven/bin/mvn test"
    }
 */
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
	sh "sudo docker tag helloworldjava:$BUILD_NUMBER 474173922354.dkr.ecr.us-east-1.amazonaws.com/tomcatapp:latest"
	sh "sudo docker push 474173922354.dkr.ecr.us-east-1.amazonaws.com/tomcatapp:latest"
    }
	
    stage('Fargate TaskDefinition Creation')
    withAWS(role:'AdminAccess-IAM-Role', roleAccount:'474173922354')
	{
	echo "${taskfamily1}"
	sh 'aws ecs register-task-definition --family ecs-fargate-cluster-svc4 --cli-input-json file://ecs-fargate-cluster-svc1.json --region us-east-1'
    }
	  
    stage('Deploy to Fargate Cluster')
    withAWS(role:'AdminAccess-IAM-Role', roleAccount:'474173922354')
	{
	echo "${taskfamily1}"
        sh "aws ecs update-service --cluster ecs-fargate-cluster-test4 --service ecs-fargate-cluster-svc4 --task-definition ecs-fargate-cluster-svc4 --desired-count 2 --region us-east-1"
    }
}
