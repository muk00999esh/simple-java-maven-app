pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
      }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
			stage("Stage with input") {
				steps {
					
					script {
						emailext (
								subject: "Deployed: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
								body: "Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]': Check console output at '${env.BUILD_URL}/input' [${env.BUILD_NUMBER}]",
								to: "muk00999esh@gmail.com"
						)
						def userInput = input(id: 'Proceed1', message: 'Promote build?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']])
						echo 'userInput: ' + userInput

						if(userInput == true) {
							sh './jenkins/scripts/deliver.sh'
						} else {
							// not do action
							echo "Action was aborted."
						}

					}    
				}  
			}
    }
}
