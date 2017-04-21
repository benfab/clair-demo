FROM jenkins:2.32.2
MAINTAINER  Benoit Fabre benoit.fabre@dell.com
COPY plugins.txt /usr/share/jenkins/plugins.txt
#COPY basic-security.groovy /var/lib/jenkins/init.groovy.d/basic-security.groovy
#COPY jenkins.install.UpgradeWizard.state /var/lib/jenkins/jenkins.install.UpgradeWizard.state
#COPY config.xml /var/jenkins_home
#COPY gradleuser-config.xml /var/jenkins_home
RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt
