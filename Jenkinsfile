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
    			script {
            			allure([
                    			includeProperties: false,
                    			jdk: '',
                    			results: [[path: '/allure-results']]
            			])
    			}
    			}
		}
	}
	post{
		always{
			archiveArtifacts artifacts: 'output/**'
			bat "docker-compose down"
		}
	}
}
