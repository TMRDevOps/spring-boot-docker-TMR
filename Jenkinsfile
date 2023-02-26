node{
     
    stage('SCM Checkout'){
        git credentialsId: 'Github', url:  'https://github.com/TMRDevOps/spring-boot-docker-TMR.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.6.1", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t tmrdevops/spring-boot-mongo .'
    }
    
    stage('Push Docker Image'){
        withCredentials([usernamePassword(credentialsId: 'Dockerhub', passwordVariable: 'dockerpass', usernameVariable: 'dockeruser')]) {
          sh "docker login -u tmrdevops -p ${dockerpass}"
        }
        sh 'docker push tmrdevops/spring-boot-mongo'
     }
     
    stage('Remove Docker Image'){
        sh 'docker rmi -f $(docker images -q)'
     }


     stage("Deploy To Kuberates Cluster"){
       kubernetesDeploy(
         configs: 'springBootMongo.yml', 
         kubeconfigId: 'KUBERNATES_CONFIG',
         enableConfigSubstitution: true
        )
     }
	 
	  /**
      stage("Deploy To Kuberates Cluster"){
        sh 'kubectl apply -f springBootMongo.yml'
      } **/
     
}


