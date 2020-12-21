Images streams update - https://access.redhat.com/documentation/en-us/net/5.0/html-single/getting_started_with_.net_on_rhel_8/index#creating-an-application-using-dotnet_using-dotnet-on-rhel

#Build Configuration
#get images list

oc get is -n openshift | awk '{ print $1 }'
oc get templates -n openshift

1.#Building Your Applications From Source Code

oc new-build nodejs~https://github.com/cesarvr/hello-world-nodejs --name-nodejs-build

oc get is

2.#Build an Application From Existing Binary

git clone https://github.com/cesarvr/Spring-Boot spring_boot
# enter the folder
cd spring_boot

#Generate the binary executable file in ./builds/libs/

gradle bootJar 

#We got a file named: hello-boot-0.1.0.jar

#Generate the build configuration

oc new-build java --name=java-binary-build --binary=true

#The last step is to trigger the build:

oc start-build bc/java-binary-build \
--from-file=./build/libs/hello-boot-0.1.0.jar \
--follow    

#3.Creating Your Own Container

# clone the app

git clone https://github.com/cesarvr/hello-world-nodejs hellojs

cd hellojs

FROM mhart/alpine-node:base-8
COPY * /run/
EXPOSE 8080
CMD ["node", "/run/app.js"]

#  cat build.Dockerfile | oc new-build --name node-container --dockerfile='-'

# We create the build by using the new-build command,  but this time, we aren't going to define a builder image to use, 
# instead, we are going to use the --dockerfile  parameter. The --dockerfile accepts a string containing Dockerfile instructions.
# If we pass a dash  --dockerfile='- ‘ , we can stream the contents of our build.Dockerfile.

# oc start-build bc/node-container --follow

#4.Exporting Images 

oc import-image microservice:latest \ 
 --from=your-docker-registry.io/project-name/cutting-edge:latest \
 --confirm

#To check your image, you just need to write this command:
# oc get is

Deploying Your Application

$oc get is

NAME                DOCKER REPO                                                               
java-binary-build   docker-registry.default.svc:5000/hello01/java-binary-build   
node-build          docker-registry.default.svc:5000/hello01/node-build          

oc new-app java-binary-build --name=java-ms

# Expose the service of our application

oc expose svc java-ms

# Now we want to know the URL. 

oc get route

NAME        HOST/PORT                                                   
java-ms     java-ms-hello01.7e14.starter-us-west-2.openshiftapps.com

# URL for the exterior

curl java-microservice-hello01.7e14.starter-us-west-2.openshiftapps.com

#Greetings from Spring Boot!


assafsabag/maxauth:2.1

oc import-image maxauth:2.1 \ 
 --from=assafsabag/maxauth:2.1 \
 --confirm

 oc import-image maxauth:2.1 --from=assafsabag/maxauth:2.1 --confirm


 https://raw.githubusercontent.com/redhat-developer/s2i-dotnetcore/master/dotnet_imagestreams.json
---------------------------------------------------------------------------------------
docker build . -t avishayx/rubic-kerb:1.0
docker push avishayx/rubic-kerb:1.0


oc delete bc,dc,is,deployment,svc,route,secret --all -n <namespace>

kubectl create secret generic keytab-secret --from-file bh.HTTP.keytab

#oc import-image rubic-image:2.1 --from=assafsabag/maxauth:2.1 --confirm
#avishayx/rubic-kerb:1.0
#assafsabag/maxauth:2.1


oc new-app --template=openjdk18-web-basic-s2i

$ oc new-app --template=openjdk18-web-basic-s2i -p APPLICATION_NAME=spring-rest \
 -p SOURCE_REPOSITORY_URL=https://github.com/redhat-cop/spring-rest.git \ 
 -p CONTEXT_DIR=''

oc new-app --name=rubic-app 'dotnet:5.0-ubi8~https://github.com/assafSabag/rubic.git' --build-env DOTNET_STARTUP_PROJECT=maxApi

1.oc new-app dotnet-runtime:latest~. --name rubic-app-test-2 --strategy=source --context-dir maxApi

2.#oc new-app rubic-image:2.1~https://github.com/assafSabag/rubic.git \
     --name rubic-app-test-2 --strategy=source --context-dir maxApi

test -- // oc new-app https://github.com/assafSabag/rubic.git --name=rubic-build-test-1 --source-secret keytab-secre --strategy=source --context-dir maxApi

----
oc new-build dotnet:5.0-ubi8~https://github.com/assafSabag/rubic.git --name=rubic-build-test-1 --strategy=source --context-dir maxApi

oc new-app rubic-build-test-1 --name=rubic-deploy-1


oc new-app --search dotnet

oc process template.yaml --param-file=production.params | oc apply -f -

#production.params file:#
DB_USER=michel
OPENSHIFT_PROJECT=teleporter-prod

apiVersion: v1
kind: Template
metadata:
  name: MyApplication
objects:

- kind: ConfigMap
  apiVersion: v1
  metadata:
    name: '${APPLICATION_NAME}'
    namespace: ${OPENSHIFT_PROJECT}
  data:
    DB_USER: ${DB_USER}

- kind: DeploymentConfig
  [...]

- kind: Service
  [...]

- kind: Route
  [...]

parameters:
- name: DB_USER
- name: APPLICATION_NAME
- name: OPENSHIFT_PROJECT



oc get templates -n openshift
mongodb-ephemeral

oc get -o yaml template mongodb-ephemeral -n openshift


oc get -o yaml --export all > <yaml_filename>




oc new-app --template=dotnet-runtime-example -p APPLICATION_NAME='rubic-rest'  -p SOURCE_REPOSITORY_URL=https://github.com/assafSabag/rubic.git -p CONTEXT_DIR=maxApi

oc new-app --template=openjdk18-web-basic-s2i -p APPLICATION_NAME=spring-rest -p SOURCE_REPOSITORY_URL=https://github.com/redhat-cop/spring-rest.git -p CONTEXT_DIR=''

oc new-app --name=rubic-rest 'dotnet:5.0-ubi8~https://github.com/assafSabag/rubic.git' --build-env DOTNET_STARTUP_PROJECT=maxApi
