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
stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Unit Tests') {
      steps {
        sh 'mvn test'
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
	         sh 'wget "https://github.com/jeremylong/DependencyCheck/releases/download/v8.4.2/dependency-check-8.4.2-release.zip" '
		sh 'unzip dependency-check-8.4.2-release.zip'
		sh 'cd dependency-check'
		sh './bin/dependency-check.sh --scan django.nV-master'
	        sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
			}
    }


}



