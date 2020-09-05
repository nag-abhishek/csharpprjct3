


pipeline {
  agent any
  environment {
   scannerHome = tool name: 'sonar_scanner_dotnet', type: 'hudson.plugins.sonar.MsBuildSQRunnerInstallation'
    registry = "abhishek3150089/netprjct"
    registryCredential = "dockrHub"
    dockerImage = ""
  }
  stages {
      stage('checkout code'){
    steps{
		echo 'Pulling...' + env.BRANCH_NAME
    git 'https://github.com/nag-abhishek/csharpproject2.git'
}
}

stage('restore nuget'){
 steps{echo GIT_BRANCH %GIT_BRANCH%
     bat 'dotnet restore WebApplication4.sln'
}
}
stage('sonarQube Analysis'){
   
    
    steps{
        script {
       
           withSonarQubeEnv("Test_Sonar") {
           bat """
           ${scannerHome}\\SonarScanner.MSBuild.exe  begin /k:"my.project" /d:sonar.host.url="http://localhost:9000" /d:sonar.login="9d26a2e7700339dd08509b738568b49c2ababd8b"
          
          """
               }
           }
    }
}
stage('build'){
steps{
bat "dotnet build WebApplication4.sln -c Release -o app/publish"
    
}
}

stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('build h') {
      steps{
       script {
       bat "docker run -p 8081:80" registry + ":$BUILD_NUMBER"""
     
          }
      }
    }
    
}
}
