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

	stage('Checkout') {
            steps {
                sh 'wget "https://github.com/SonarSource/sonar-scanning-examples/tree/master/sonarqube-scanner" '
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
	         sh 'rm owasp* || true'
	         sh 'wget "https://raw.githubusercontent.com/brunokaique/SAST-Android/master/owasp-dependency-check.sh" '
	         sh 'chmod +x owasp-dependency-check.sh'
	         sh 'bash owasp-dependency-check.sh'
	        sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
			}
    }


}


}
