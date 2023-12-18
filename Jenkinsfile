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
                  
                 dotnetSteps.build()
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests for your .NET application
                   dotnetSteps.test()
                }
            }
        }

        stage('Publish') {
            steps {
                script {
                    // Publish your .NET application if needed
                    dotnetSteps.publish()
                }
            }
        }

         stage('SonarQube Analysis') {
             steps{

                 script{
      
            withSonarQubeEnv() {
          dotnetsonarqube.scan("mvp-dotnet")
            }
                 }
             }
  }

        stage('zip artifact'){
        steps{
            script{

                sh 'tar -czvf artifact.tar.gz bin/Release/net6.0'
    

            }


        }


        }
        stage('Deploy to Nexus') {
        steps {
            script {

            withCredentials([string(credentialsId: 'nexusurl', variable: 'NEXUS_URL'), string(credentialsId: 'mvp-dotnet-nexus-id', variable: 'NEXUS_REPO_ID'), string(credentialsId: 'nexuspassword', variable: 'NEXUS_PASSWORD'), string(credentialsId: 'nexususername', variable: 'NEXUS_USERNAME')]) {
        dotnetNexusTask.pushToNexus(NEXUS_USERNAME, NEXUS_PASSWORD, NEXUS_URL, NEXUS_REPO_ID, PACKAGE_NAME)
          
            }
            }
        }
    }

        stage('Build and Push Docker Image') {
            steps {
                script {

                dockertask.buildAndPush(env.IMAGE_NAME, env.BUILD_ID, env.DOCKERFILE_PATH, env.DOCKER_HUB_CREDENTIALS)
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
 stage('Trigger Downstream Pipeline') {
            steps {
                script {
                    // Trigger the downstream pipeline (Pipeline B) only if the build is stable or successful
                    if (currentBuild.resultIsBetterOrEqualTo('SUCCESS')) {
                        build job: 'AutoDeploytoDev-dotnet', parameters: [string(name: 'buildID', value: env.BUILD_ID)]
                    } else {
                        echo 'Skipping downstream pipeline due to unsuccessful upstream build.'
                    }
                }
            }
        }
    }
}
