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
  }
}
