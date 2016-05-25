Jenkins PHPCI docker image with aws / docker / rancher tools installed

default password is 'changeit2016!'. You must setup your data volumes after the creation of the jenkins server.

* docker cp <jenkins-phpci-primary>:/var/lib/jenkins temp-jenkins
* docker cp temp-jenkins <jenkins-phpci-data>:/var/lib/jenkins
* start the services
