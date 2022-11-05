node{
    stage('Source code get from github'){
        git 'https://github.com/janaganp/my-app'
    }
    stage('code built in maven'){
        def mvnHome = tool name:'maven3', type:'maven'
        sh "${mvnHome}/bin/mvn clean package"
        sh 'mv target/myweb*.war target/newapp.war'
    }
     stage('Docker build images'){
        sh 'docker build -t janaganp/tomimage .'
    }
    stage('Docker image push in the dockerhub'){
    withCredentials([string(credentialsId:'dockerPass', variable:'dockerPassword')]){
        sh "docker login -u janaganp -p ${dockerPassword}"
    }
    sh 'docker push janaganp/tomimage'
    }
	stage('Nexus Image Push'){
        sh "docker login -u admin -p admin123 18.217.49.179:8095"
        sh "docker tag janaganp/tomimage 18.217.49.179:8095/jana:1.0.0"
        sh 'docker push 18.217.49.179:8095/jana:1.0.0'
   }
    stage('Remove Previous Container'){
	try{
		sh 'docker rm -f webhosting'
	}catch(error){
		//  do nothing if there is an exception
	}
    }
    stage('docker deployment'){
        sh 'docker run -itd --name webhosting -p 8090:8080 janaganp/tomimage'
    }
}
