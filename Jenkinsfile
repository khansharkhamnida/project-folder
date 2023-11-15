

pipeline {
    agent {
        docker {
            image 'node:20.9.0-alpine3.18'
            args '-p 3002:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'chmod +x ./Jenkins/scripts/test.sh'
                sh './Jenkins/scripts/test.sh'
            }
        }
        stage('Deliver') { 
            steps {
		sh 'chmod +x ./Jenkins/scripts/deliver.sh'
                sh './Jenkins/scripts/deliver.sh' 
                input message: 'Finished using the web site? (Click "Proceed" to continue)' 
                sh './Jenkins/scripts/kill.sh' 
            }
        }
        stage('OWASP Dependency-Check Vulnerabilities') {
              steps {
                dependencyCheck additionalArguments: ''' 
                            -o './'
                            -s './'
                            -f 'ALL' 
                            --prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
                
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
              }
        }
    }
}

