node {
    // reference to maven
    // ** NOTE: This 'maven-3.6.1' Maven tool must be configured in the Jenkins Global Configuration.   
    def mvnHome = tool 'maven-3.8.5'

    // holds reference to docker image
    def dockerImage
    // ip address of the docker private repository(nexus)
    
    def dockerRepoUrl = "1.2.3.4"
    def dockerImageName = "hello-world-java"
    def dockerImageTag = "${dockerRepoUrl}/${dockerImageName}:${env.BUILD_NUMBER}"
    
    stage('Clone Repo') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/dstar55/docker-hello-world-spring-boot.git'
      // Get the Maven tool.
      // ** NOTE: This 'maven-3.6.1' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven-3.8.5'
    }    
  
    stage('Build Project') {

      sh "git pull"    
	    
      // build project via maven
      sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean install"
    }
	
    stage('Build Docker Image') {
      // build docker image
      sh "whoami"
      //sh "ls -all /var/run/docker.sock"
      sh "mv ./target/hello*.jar ./data" 
      
      //dockerImage = docker.build("hello-world-java")
      dockerImage = docker.build("hello-world-java", "-f", "Dockerfile", "-t", "${dockerImageTag}")
    }
   
    stage('Deploy Docker Image'){

      echo "Docker Image Tag Name: ${dockerImageTag}"
      sh 'docker stop my-container'
      sh 'docker rm my-container'
      sh 'docker run -d -p 8082:8080 --name my-container hello-world-java'
    }
}
