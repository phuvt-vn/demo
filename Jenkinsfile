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
      git 'https://github.com/phuvtvo/demo.git'
      // Get the Maven tool.
      // ** NOTE: This 'maven-3.6.1' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven-3.8.5'
    }    
  
    stage('Build Project') {

      sh "git branch --set-upstream-to=origin/master master"    
	    
      // build project via maven
      sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean install"
    }
	
    stage('Build Docker Image') {
      // build docker image
      sh "whoami"
      sh "ls -all /var/run/docker.sock"
      sh "mv ./target/hello*.jar ./data" 
      sh "ls -all /var/run/docker.sock"
      sh "docker rmi hello-world-java"

      	
	sh 'current_dir=$(pwd)'

	
	sh 'echo "$current_dir/data/hello*.jar"'
	    
      dockerImage = docker.build("hello-world-java")

    }
   
    stage('Deploy Docker Image'){

      echo "Docker Image Tag Name: ${dockerImageTag}"
      sh 'docker stop my-container'
steps {
	script {
	    def imageName = 'hello-world-java'

	    // Check if the image exists
	    def imageExists = sh(script: "docker inspect -f '{{.Id}}' $imageName", returnStatus: true) == 0

	    if (imageExists) {
		// Image exists, remove it
		sh "docker rmi $imageName"
	    } else {
		// Image doesn't exist, return true (or take appropriate action)
		echo "Image $imageName does not exist."
		return true
	    }
	}
}	
      sh 'docker rm my-container'
      sh 'docker run -d -p 8082:8080 --name my-container hello-world-java'
    }
}
