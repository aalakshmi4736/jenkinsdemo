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
               bat '''for pid in $(lsof -t -i:9090); do
                       kill -9 $pid
               done'''
               bat 'cd WebApplication/bin/Release/netcoreapp3.1/publish/'
               bat 'nohup dotnet WebApplication.dll --urls="http://20.123.27.87:80" --ip="20.123.27.87" --port=80 --no-restore > /dev/null 2>&1 &'
             }
        }
  }
}
