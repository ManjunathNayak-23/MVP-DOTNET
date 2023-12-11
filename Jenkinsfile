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
    def scannerHome = tool 'sonarqubeMS'
   withSonarQubeEnv() {
      sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll begin /k:\"mvp-dotnet\""
      sh "dotnet build"
      sh "dotnet ${scannerHome}/SonarScanner.MSBuild.dll end"
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
