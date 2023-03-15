node{
   stage('SCM Checkout'){
     git 'https://github.com/karthicksreenivas/my-app.git'
   }
   stage('maven-buildstage'){
      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
    stage('SonarQube Analysis') {
      def mvnHome =  tool name: 'maven3', type: 'maven'
      withSonarQubeEnv('sonar') { 
      sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Image'){
   sh 'docker build -t karthi/newapp .'
   }
    stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u karthicksreenivas -p ${dockerPassword}"
   sh "docker tag karthi/newapp karthicksreenivas/pipe:my-img"
   sh 'docker push karthicksreenivas/pipe:my-img'
   }
    }
     stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 13.233.80.215:8085"
   sh "docker tag karthi/newapp 13.233.80.215:8085/nex1:01"
   sh 'docker push 13.233.80.215:8085/nex1:01'
   }   
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattty53'
	}catch(error){
		//  do nothing if there is an exception
	}
   }
    stage('Docker deployment'){
   sh 'docker run -d -p 8015:8080 --name tomcattty53 karthi/newapp' 
    }
   }
