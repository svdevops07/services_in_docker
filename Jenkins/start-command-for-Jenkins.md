# LTS JDK11:
docker run --name jenkins-cont -p 8080:8080 -p 50000:50000 --restart always jenkins/jenkins:lts-jdk11

# LTS JDK11 with volume '/var/jenkins_home':
docker run --name jenkins-cont -p 8080:8080 -p 50000:50000 --restart always -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk11