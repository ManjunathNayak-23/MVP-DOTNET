@Library('sharedLibrary') _
pipeline {
    agent any
 environment {
     DOCKER_HUB_CREDENTIALS = 'dockercred' // Replace with your Docker Hub credentials ID
     IMAGE_NAME = 'chandu2311/mvpdotnet'
     DOCKERFILE_PATH = 'Dockerfile'
     PACKAGE_NAME = 'mvp-dotnet'
 
  }
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
             steps{

                 script{
        def scannerHome = tool 'sonarqubeMS'
           
          dotnetsonarqube.scan("mvp-dotnet")
                 }
             }
  }

//         stage('zip artifact'){
//         steps{
//             script{

//                 sh 'tar -czvf artifact.tar.gz bin/Release/net6.0'
    

//             }


//         }


//         }
//         stage('Deploy to Nexus') {
//       steps {
//         script {

//           withCredentials([string(credentialsId: 'nexusurl', variable: 'NEXUS_URL'), string(credentialsId: 'mvp-dotnet-nexus-id', variable: 'NEXUS_REPO_ID'), string(credentialsId: 'nexuspassword', variable: 'NEXUS_PASSWORD'), string(credentialsId: 'nexususername', variable: 'NEXUS_USERNAME')]) {
//  def curlCommand = """
// curl -v -u ${NEXUS_USERNAME}:${NEXUS_PASSWORD} --upload-file artifact.tar.gz ${NEXUS_URL}/repository/${NEXUS_REPO_ID}/${PACKAGE_NAME}/${PACKAGE_NAME}-${env.BUILD_ID}.tar.gz         """
//  sh curlCommand
    
//           }
//         }
//       }
//     }

// stage('Build and Push Docker Image') {
//       steps {
//         script {

//           dockertask.buildAndPush(env.IMAGE_NAME, env.BUILD_ID, env.DOCKERFILE_PATH, env.DOCKER_HUB_CREDENTIALS)
//         }
//       }
//     }
 

     
//         stage('OWASP Dependency-Check Vulnerabilities') {
//       steps {
//         script {
//           dependencyCheckTask.owaspDependencyCheck()
//         }
//       }
//     }

        
    }
}
