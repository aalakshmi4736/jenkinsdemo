pipeline {
   agent any
	 triggers {
        githubPush()
    }
   stages {
   stage('Restore packages'){
	   steps{
		   bat 'dotnet restore WebApplication.sln'
		}
	 }
   stage('Build'){
           steps{
               bat 'dotnet build WebApplication.sln --configuration Release --no-restore'
            }
         }
  
  stage('Publish'){
             steps{
               bat 'dotnet publish WebApplication/WebApplication.csproj --configuration Release --no-restore'
             }
        }
   stage('Deploy'){
             steps{
                sh '''for pid in $(lsof -t -i:9090); do
                       kill -9 $pid
               done'''
               sh 'cd WebApplication/bin/Release/netcoreapp3.1/publish/'
               sh 'nohup dotnet WebApplication.dll --urls="http://20.123.27.87:9090" --ip="20.123.27.87" --port=9090 --no-restore > /dev/null 2>&1 &'
             }
        }
  }
}
