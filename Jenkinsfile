// sudo usermod aG docker jenkins
node {
    def application="springbootapp"
    def dockerhubaccountid="tridevg"

    stage('clone repo') {
      sh 'hostname -f'
      git branch: 'main', url: 'https://github.com/adsnjhfyeqw231eas/springbootapp.git'
    }

    stage('build image') {
      app=docker.build("${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
    }

	stage('docker login image') {
	  withDockerRegistry([ credentialsId: "dockercred", url: "" ]) {
        echo 'mic testing'
		app.push()
		app.push('latest')
	  }
	}

    stage('deploy') {
      sh ("docker run -d -p 80:8080 -v /var/log/app:/var/log/app --name app-${BUILD_NUMBER} ${dockerhubaccountid}/${application}:${BUILD_NUMBER}")
  }
    stage('verify container') {
      sh 'docker ps'
  }

	stage('Remove old images') {
		// remove docker pld images
		sh("docker rmi ${dockerhubaccountid}/${application}:latest -f")
   }

}
