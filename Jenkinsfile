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
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP - Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.login=sqp_5360cbbcf29e1faba7416907658a4a45a44e25e4"
                    }
                }
            }
        }
    }
    post {
        always {
            junit testResults: '**/target/surefire-reports/TEST-*.xml'
            recordIssues enabledForFailure: true, tools : [mavenConsole(), java(), javaDoc()]
            recordIssues enabledForFailure: true, tools : [checkStyle()]
            recordIssues enabledForFailure: true, tools : [spotBugs(pattern: '**/target/findbugsXml.xm1')]
            recordIssues enabledForFailure: true, tools : [cpd(pattern: '**/target/cpd.xml')]
            recordIssues enabledForFailure: true, tools : [pmdParser(pattern: '**/target/pmd.xml')]
        }
    }
}


