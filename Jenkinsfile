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
	          stage('Install Trivy') {
            steps {
                sh 'sudo apt update'
                sh 'sudo apt install wget'
                sh 'wget https://github.com/aquasecurity/trivy/releases/download/v0.19.1/trivy_0.19.1_Linux-64bit.tar.gz'
                sh 'tar zxvf trivy_0.19.1_Linux-64bit.tar.gz'
                sh 'sudo mv trivy /usr/local/bin/'
            }
        }
	          stage('Build Docker Image') {
            steps {
                // Replace with your Docker build steps
                sh 'docker build -t my-docker-image:latest .'
            }
        }

        stage('Scan Docker Image with Trivy') {
            steps {
                script {
                    def imageName = 'my-docker-image:latest'
                    def trivyCmd = "trivy --severity HIGH --ignore-unfixed ${imageName}"
                    def trivyScan = sh(script: trivyCmd, returnStatus: true)
                    if (trivyScan != 0) {
                        currentBuild.result = 'FAILURE'
                        error "Trivy found vulnerabilities"
                    }
                }
	    }
	}

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
}



