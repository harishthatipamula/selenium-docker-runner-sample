pipeline{
	agent any
	stages{
		stage("Pull Latest Image"){
			steps{
				bat "docker pull abhishekneela/b2b-automation-docker"
			}
		}
		stage("Start Grid"){
			steps{
				bat "docker-compose up -d hub chrome firefox allure allure-ui"
			}
		}
		stage("Run Test"){
			steps{
				bat "docker-compose up search-module book-flight-module"
			}
		}
		stage('reports') {
    			steps {
					allure includeProperties: false, jdk: '', results: [[path: 'target/allure-results']]
    			}
		}
	}
	post{
		always{
			
            unstash 'allure-results' //extract results
            script {
                allure([
                includeProperties: false,
                jdk: '',
                properties: [],
                reportBuildPolicy: 'ALWAYS',
                results: [[path: 'allure-results']]
            ])
            }

			archiveArtifacts artifacts: 'output/**'
			bat "docker-compose down"
		}
	}
}
