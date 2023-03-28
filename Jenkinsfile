pipeline{
	agent any
	stages{
		stage("Pull Latest Image"){
			steps{
				bat "docker pull happyharish/b2b-automation-docker"
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
					stash name: 'allure-results', includes: '/usr/share/udemy/test-output/allure-results/*'
                   
				
				
			}
		}
		// stage('Generate Report') {
		// 	steps {
		// 			sh './node_modules/.bin/allure generate ./allure-results'
		// 	}
		// }
		// stage('reports') {
    	// 		steps {
		// 			allure includeProperties: false, jdk: '', results: [[path: 'target/allure-results']]
    	// 		}
		// }
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
