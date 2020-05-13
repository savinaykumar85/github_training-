*********************Dockerfile*****************
FROM centos
RUN yum install -y java
RUN yum install -y wget
RUN wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
RUN rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
RUN yum install git -y
RUN echo "jenkins ALL=(ALL)	NOPASSWD:ALL" >> /etc/sudoers
RUN yum install -y jenkins
EXPOSE 8080
CMD ["java", "-jar", "/usr/lib/jenkins/jenkins.war" ]

******************************job1*****************
cp . -r -v -f /var/www/html/
********************job2***************************
yum install -y httpd
/usr/sbin/httpd
cp . -r -v -f /var/www/html/
*******************job3****************************
status=$(curl -o /dev/null -s -w "%{http_code}" 10.128.0.11:80/index.html)

if [[ $status == 200 ]]
then
 echo "webApp is working"
      exit 1
else
 echo "webApp is not working"
      exit 1
fi

**********************Job4*****************************
chroot /host /bin/bash <<"EOT"
if docker ps | grep web_server
then 
exit 0
else
docker rm -f web_server
docker run -itd -p 1234:80 -v /root/mywebdata:/var/www/html apache:v1
**************************************************************
