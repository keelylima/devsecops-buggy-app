pipeline {
  agent any
  tools { 
        maven 'Maven_3_5_2'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=security-keri-buggy-app_devsecopsapp -Dsonar.organization=security-keri-buggy-app -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=be0a88c9166e0e553d6936a6ae5df5a6bc44ab4e'
			}
        } 
    stage('RunSCAAnalysisUsingSnyk') {
            steps {	
		withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
			// Esse -fn é para evitar falha no build, sabemos que tem vulnerabilidade mas queremos que ele complete o build mesmo assim
			sh 'mvn snyk:test -fn'
		}
	   }
        }

    stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("devsecops-docker")
                 }
               }
            }
        }
    stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://695680868060.dkr.ecr.us-east-1.amazonaws.com/devsecops', 'ecr:us-east-1:aws-credentials-devsecops') {
                    app.push("latest")
                    }
                }
            }
    	}
    }
}
