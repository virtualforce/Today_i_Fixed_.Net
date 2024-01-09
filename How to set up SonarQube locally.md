# How to set up Sonarqube locally? 
# What is SonarQube
SonarQube is a Code Quality Assurance tool that collects and analyzes source code and provides reports on the code quality of our project. It ensures code reliability, detects bugs, and alerts developers to other source code issues.
# Problem
How do we ensure that we're writing quality code?
# Environment
Visual Studio 

# Here are the necessary steps to set up SonarQube locally
By following these steps, you will be able to set SonarQube with your projects:
* First, you need to add SonarLint Extension in Visual Studio.
* Then download the latest Java JDK from here https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html
* After that, download the SonarQube from here https://www.sonarsource.com/products/sonarqube/downloads/
* Then download SonarQube Scanner .Net from here according to the .Net version you are using: https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/scanners/sonarscanner-for-dotnet/
* Now add java bin and sonar scanner Path to environment variable. 
# How to integrate any project with SonarQube for the report?
By following these steps, you will be able to integrate SonarQube with your projects:
* Go to http://localhost:9000/ (B)
* Create a user and password. (by default, user name and password are ``admin``).
* Create a new project and generate the token.
* Then select .Net/.Net Core.
* You will see three commands there. (A)
* Now Open Visual Studio, then click on the extension from the title bar.
* From extension select SonarLint >> Connected mode >> bind to sonnarqube or sonar cloud.
* A new menu will appear; from this menu, click on connect and enter the server url, user and password. (server url  http://localhost:9000/ )
* you will now see the list of projects you created on SonarQube Server.
* Select the project that you have created.
* Now open the command prompt in your local project repository and run those three commands as in step A.
```
step 1
C:\Sonar\Scanner\SonarScanner.MSBuild.exe begin /k:"FirstCrustReporting" /d:sonar.host.url="http://localhost:9000" /d:sonar.login="235549ed27ec5fa86ff16bcc1b4944afbd364f6f"

step 2

"C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin\"MsBuild.exe /t:Rebuild

step 3

C:\Sonar\Scanner\SonarScanner.MSBuild.exe end /d:sonar.login="235549ed27ec5fa86ff16bcc1b4944afbd364f6f"
```
Here ``FirstCrustReporting`` is your project name and ``235549ed27ec5fa86ff16bcc1b4944afbd364f6f``
* You will notice I have added "C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin\" in front of 2nd command. You also have to find the path of this bin file and add it in front of the 2nd command in A.
* Your report is ready to view on the SonarQube Server in B.