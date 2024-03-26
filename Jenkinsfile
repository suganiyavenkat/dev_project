node{
   stage('SCM CHECKOUT'){
     git 'https://github.com/SundaramWild/Project.git'
   }
      stage('BUILD-MAVEN'){
      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
      stage('DOCKER IMAGE BUILD'){
   sh 'docker build -t sundaramwild/myweb:0.0.5 .'
   }
      stage('DOCKERHUB PUSH'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u sundaramwild -p ${dockerPassword}"
    }
   sh 'docker push sundaramwild/myweb:0.0.5'
   }
      stage('REMOVE PREVIOUS CONTAINER'){
	try{
		sh 'docker rm -f tomcat'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('DOCKER DEPLOYMENT'){
   sh 'docker run -d -p 8090:8080 --name tomcat sundaramwild/myweb:0.0.5' 
   }
}
}
