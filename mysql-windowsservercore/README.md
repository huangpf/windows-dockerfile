# Description:

Creates and image containing mysql 5.6.29. Using this example, the root password will be blank, and remote connections enabled. There are a few hacks in this Dockerfile to mitigate some unknown issues. The dockerfile CMD is just a persistent ping to give the container something to hang off of.

This dockerfile is for demonstration purposes and may require modification for production use. 

# Environment:

Windows Server Core Base OS Image

# Usage:

**Docker Build**

`Docker Build –t mysql .`

**Docker Run**

`docker run -d -p 80:80 mysql`

# Dockerfile Details:
```
# This dockerfile utilizes components licensed by their respective owners/authors.
# Prior to utilizing this file or resulting images please review the respective licenses at: http://www.mysql.com/about/legal/licensing/oem/

FROM windowsservercore

MAINTAINER neil.peterson@microsoft.com

LABEL Description="MySql" Vendor="Oracle" Version="5.6.29"

RUN powershell -Command \
	Invoke-WebRequest -Method Get -Uri https://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.29-winx64.zip -OutFile c:\mysql.zip ; \
	Expand-Archive -Path c:\mysql.zip -DestinationPath c:\ ; \
	Remove-Item c:\mysql.zip -Force

RUN SETX /M Path %path%;C:\mysql-5.6.29-winx64\bin

RUN powershell -Command \
	mysqld.exe --install ; \
	Start-Service mysql ; \
	Stop-Service mysql ; \
	Start-Service mysql

RUN mysql -u root -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '' WITH GRANT OPTION;"

CMD [ "ping localhost -t" ]


```

