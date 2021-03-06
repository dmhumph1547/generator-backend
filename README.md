# Code Generator Backend

[![Build Status](https://travis-ci.org/obsidian-toaster/generator-backend.svg?branch=master)](https://travis-ci.org/obsidian-toaster/generator-backend)

This code generator project which is a Java backend system exposes several JBoss Forge commands
using a REST endpoint. The backend runs within a WildFly Swarm container and is called from
an Angularjs 2 Front application responsible to collect from a end user the information needed to generate
a Zip file containing an Apache Maven project populated for an Eclipse Vert.x, Spring Boot or WildFly Swarm
container.

To execute this project simply do a maven build:

```bash
$ mvn package -s configuration/settings.xml
```

Remark : This project requires that you compile this [github project](http://github.com/obsidian-toaster/obsidian-addon) hosting the Obsidian JBoss Addon.

And then execute the fat-jar in the target folder with:

```bash
$ java -jar target/generator-swarm.jar
```

Then follow the [front-end ReadMe][1] to run the front-end.

[1]:https://github.com/obsidian-toaster/generator-frontend/blob/master/README.md

To deploy the backend generator on OpenShift, run this command within the terminal

```
oc new-project backend
oc create -f templates/backend-template.yml
oc process backend-generator-s2i | oc create -f -
oc start-build generator-backend-s2i
```

Redeploy the application with:

```
mvn fabric8:deploy -Popenshift
```

You can verify now that the service replies

```
http $(minishift service generator-backend --url=true)/forge/version
curl $(minishift service generator-backend --url=true)/forge/version
```

## HTTPS support

The JAR also runs in SSL. The Keystore was created using:
```
keytool -genkeypair -alias appserver -storetype jks -keyalg RSA -keysize 2048 -keypass password -storepass password -dname "CN=generator-backend-obsidian.e8ca.engint.openshiftapps.com,OU=Engineering,O=Red Hat Inc,L=Raleigh NC,C=US" -validity 730 -v -keystore keystore.jks
```

