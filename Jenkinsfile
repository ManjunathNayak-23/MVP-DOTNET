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
        SCANNER_HOME = tool 'Sonar-scanner'
      }
            steps {
                // Run SonarScanner for .NET
                script {
                    SONARQUBE_SERVER='http://34.42.7.89:9000/'
                    SONARQUBE_TOKEN='sonartoken'
                       
                    sh "dotnet sonarscanner begin /k:\"YourProjectKey\" /d:sonar.host.url=${SONARQUBE_SERVER} /d:sonar.login=${SONARQUBE_TOKEN}"
                    sh 'dotnet build'
                    sh 'dotnet sonarscanner end /d:sonar.login=${SONARQUBE_TOKEN}'
                    
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
