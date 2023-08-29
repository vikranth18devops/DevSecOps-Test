pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('Sonar-Analysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=aarvik_aarvikproject1 -Dsonar.organization=aarvik -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=3fb076ab2e33c231c1193a2285fa731d7831722b'
			}
}

	stage('Synk-Analysis') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }		
	stage('Docker-Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("asg")
                 }
               }
            }
    }

	stage('Docker-Push-ECR') {
            steps {
                script{
                    docker.withRegistry('https://262418941258.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	}
	    
  }
}
