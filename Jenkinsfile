node{

    stage('SCM Checkout'){
        git url:  'https://github.com/eagunuworld/mongodb-springboot.git',branch: 'development'
    }

    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "maven:3.8.5", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"

    }


    stage('Build Docker Image'){
        sh 'docker build -t dockerhandson/spring-boot-mongo .'
    }

    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'DOKCER_HUB_PASSWORD', variable: 'DOKCER_HUB_PASSWORD')]) {
          sh "docker login -u dockerhandson -p ${DOKCER_HUB_PASSWORD}"
        }
        sh 'docker push dockerhandson/spring-boot-mongo'
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
