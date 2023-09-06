

Pre-requisites:
-JBang: JBang must be installed on your machine. See instructions on how to download and install the JBang: https://www.jbang.dev/download/ or run this command to install the latest version:

curl -Ls https://sh.jbang.dev | bash -s - app setup

source ~/.bashrc

jbang version

Steps:

# Scaffold a new Camel project
Select **Karavan: Create Application** from the context menu in the VSCode editor.

    • Select Quarkus for the runtime, choose Quarkus for the runtime and enter the groupId, artifactId, and version 
    • See a new file, application.properties

# Create the Camel integration route and the REST API: 
Select **Karavan: Create Integration** from the context menu in the VSCode editor.

Open the Karavan editor (Karavan: Open) and set up:

   **REST DSL**

        - REST Configuration: to deploy in OpenShift only "platform-http" works, in case of jetty or http-netty : java.lang.IllegalArgumentException: Component netty is not a RestConsumerFactory.
        - REST: set up path, URI (direct to Camel route), bindingMode off, produces, consumes.

   **Camel Route**

        -  Set body with the Response string


# Deploy the service: 

   **Locally**

        - Karavan: Run
        
        or

        - jbang -Dcamel.jbang.version=3.21.0 camel@apache/camel run *


   **OpenShift**

   Open a terminal and sign into your OpenShift cluster
   Open and configure the application.properties setting the namespace parameter with yours value

        - Karavan: Deploy
        
        or

        - jbang -Dcamel.jbang.version=3.21.0 camel@apache/camel export --fresh  --directory={PROJECT_PATH}/.export && mvn clean package -f .export

Wait for the container image build to finish
Congratulations! You successfully created a s2i build and built container images. The image is now deployed on your Openshift


# TESTING: 
curl --location --request POST 'http://mocktoken-appdev-integration.apps.ocp4.quitala.eu/token' \
--header 'Content-Type: application/json' \
--data-raw '{
"token": "test"
}'