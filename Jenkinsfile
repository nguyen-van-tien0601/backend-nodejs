pipeline{
  agent any
tools {
  maven 'Maven'
}
  stages {
    stage ('Initialaze') {
      steps {
        sh 'echo "PATH = ${PATH}"'
}
}

  

	  	stage ('check-Secret') {
	    steps {
	    sh 'rm trufflehog || true'
	    sh 'docker run trufflesecurity/trufflehog git https://github.com/trufflesecurity/trufflehog.git > trufflehog'
	    sh 'cat trufflehog'
}
  }


        stage ('SAST') {
      steps {
      withSonarQubeEnv('sonar') {
	sh 'mvn sonar:sonar'
	sh 'cat target/sonar/report-task.txt'
      }
    }}



	      stage ('Source-Composition-Analysis'){
		steps{
	         sh 'sudo apt-get install wget apt-transport-https gnupg lsb-release '
		sh 'wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -'
		sh 'echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list'
		sh 'sudo apt-get update'
	        sh 'sudo apt-get install trivy'
		sh 'trivy fs django.nV-master'
			}
    }


}
}



