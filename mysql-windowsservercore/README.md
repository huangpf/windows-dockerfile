# Description:

Creates and image containing mysql 5.6.29. The database root account has a blank password. I am working on extending this so that basic DB administration such as creating a user, enabling remote connectivity can be completed for the image.

# Environment:

Windows Server Core Base OS Image

# Usage:

**Docker Build**
Docker Build –t mysql .

## Dockerfile Details:
```
FROM windowsservercore

RUN powershell -Command \
	Sleep 2 ; \
	Invoke-WebRequest -Method Get -Uri https://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.29-winx64.zip -OutFile c:\mysql.zip ; \
	Expand-Archive -Path c:\mysql.zip -DestinationPath c:\ ; \
	Remove-Item c:\mysql.zip -Force

RUN SETX /M Path %path%;C:\mysql-5.6.29-winx64\bin

RUN powershell -Command \
	Sleep 2 ; \
	mysqld.exe --install ; \
	Start-Service mysql
```

