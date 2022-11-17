pipeline { 
    agent any 
    stages {
        stage ('Checkout') { 
            steps {
                git branch:'master', url: 'https://github.com/OWASP/Vulnerable-Web-Application.git'
            }
        }
        stage('Code Quality Check via SonarQube') { 
            steps {
                script { 
                    def scannerHome = tool 'SonarQube'; 
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner \
  -Dsonar.projectKey=OWASP \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://192.168.1.118:9000 \
  -Dsonar.login=sqp_5360cbbcf29e1faba7416907658a4a45a44e25e4"
                    }
                }
            }
        }
    }
    post {
        always {
			recordIssues enabledForFailure: true, tool: sonarQube()
		}

    }
}


