@Library('sharedLibrary') _
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                  
                    sh 'dotnet restore'
                    sh 'dotnet build'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests for your .NET application
                    sh 'dotnet test'
                }
            }
        }

        stage('Publish') {
            steps {
                script {
                    // Publish your .NET application if needed
                    sh 'dotnet publish -c Release'
                }
            }
        }

         stage('SonarQube Analysis') {
           environment {
        SCANNER_HOME = tool 'sonarqubeMS'
      }
            steps {
                // Run SonarScanner for .NET
        
                script {
                    SONARQUBE_SERVER='http://34.42.7.89:9000/'
                    SONARQUBE_TOKEN='sonartoken'
                       
                 withSonarQubeEnv('Sonar') {
                sh '  dotnet sonarscanner begin /k:"HelloWorld" /d:sonar.host.url="http://34.42.7.89:9000/" /d:sonar.login="admin" /d:sonar.password="admin" '

                   sh 'dotnet build'
                sh 'dotnet sonarscanner end /d:sonar.login="admin" /d:sonar.password="admin"'
                }
                    
                }
            }
        }
 

     
        stage('OWASP Dependency-Check Vulnerabilities') {
      steps {
        script {
          dependencyCheckTask.owaspDependencyCheck()
        }
      }
    }

        
    }
}
