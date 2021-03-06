JBoss BRMS Loan Realtime Decision Server Demo
=============================================
This demo project will provide you with an example of creating, deploying and leveraging a set of rules
(decision table) in a Realtime Decision Server. You will be given examples of calling the rules as if
using it from an application with the RestAPI that is exposed.

There are two options for you to install this project: local and Docker.

Software
--------
The following software is required to run this demo:
- [JBoss EAP 7.0 installer](https://developers.redhat.com/download-manager/file/jboss-eap-7.0.0-installer.jar)
- [JBoss BRMS 6.4.0.GA deployable for EAP 7](https://developers.redhat.com/download-manager/content/origin/files/sha256/14/148eb9be40833d5da00bb6108cbed1852924135d25ceb6c601c62ba43f99f372/jboss-brms-6.4.0.GA-deployable-eap7.x.zip)
  - [7-Zip](http://www.7-zip.org/download.html) (Windows only): to overcome the Windows 260 character path length limit, we need 7-Zip to unzip the BRMS deployable.

Option 1 - Install on your machine
----------------------------------
1. [Download and unzip.](https://github.com/jbossdemocentral/brms-loan-realtime-decision-server-demo/archive/master.zip)

2. Add products to installs directory.

3. Run 'init.sh' or 'init.ps1' file.

4. Start JBoss BRMS Server by running ./target/jboss-eap-6.4/bin/standalone.sh

5. Login to http://localhost:8080/business-central

    ```
    - login for admin and analyst roles (u:brmsAdmin / p:jbossbrms1!)
    ```
6. Project has simple data model (Loan & Applicant) and single decision table credit score rule set.

7. Build and deploy version 1.0 of project. Click on "Open Project Editor", and in the project editor click on "Build -> Build and Deploy".

8. View Authoring -> Artifact repository to see deployed loandemo-1.0.jar artifact.

9. Open "Execution Servers" perspective via menu Deploy -> Execution Servers

10. The view shows one registered Decision Server 'local-server-123'. We will provision a container on this server which will serve our loandemo rules project. Click on the '+' sign on the right of the Decision Server and enter the following details in the pop-up:

  - Name: container-loan1.0

  - search button gathers all artifacts available, SELECT loandemo-1.0 to auto-fill rest of fields (group name, artifact id and version)

  - click on OK

11. The container definition is created, but is not yet started. Click on the "Start" button to start the container instance on the Decision Server.

12. Click on the container to show additional information about the container.

13. Using Firefox + RESTClient you can see which server containers are available by:

   - Add auth credentials in menu Authentication - Basic Authentication:  Username: brmsAdmin    Password: jbossbrms1!

   - Method: GET

   - URL: http://localhost:8080/kie-server/services/rest/server/containers

   - it will show container = contianer-loan1.0, meaning our container is available via the provided RESTful API

14. You can view some more information provided by the RESTful API using GET methods:

   - http://localhost:8080/kie-server/services/rest/server/containers/container-loan1.0

15. A full description of all available RESTful resources and operations exposed by the Decision Server can be found by opening this URL: http://localhost:8080/kie-server/docs

16. Now to use POST or PUT methods we need to add a header to RESTClient for our requests:

   - in menu Headers -> Custom Header

   - Name: Accept; Value: application/xml

   - Name: Content-Type; Value: application/xml

   - Name: X-KIE-ContentType; Value: xstream

17. Query the Realtime Decision Server with loan rules by using POST method:

   - http://localhost:8080/kie-server/services/rest/server/containers/instances/container-loan1.0

   - body of message can be found in support/loan-query.xml file, copy into Body section of RESTClient.

   - note you can adjust the credit score field in the xml message body to show rows in decision table being used.

18. You can change the decision table as desired, redeploy a new version, use the Version Configuration tab of the container definition to manage the container using UPGRADE button to pull the latest version.

   - you need to deploy a new version of the rules, for example version 1.1, then enter 1.1 in version field of container-loan1.0 before hitting UPGRADE button.

19. For creation or deletion of containers in the RESTful API, you need to use PUT methods, see product documentation User Guide for details.


Option 2 - Run in Docker
-----------------------------------------
The following steps can be used to configure and run the demo in a container

1. [Download and unzip.](https://github.com/jbossdemocentral/brms-realtime-decision-server-demo/archive/master.zip)

2. Add product installer to installs directory.

3. Run the 'init-docker.sh' or 'init-docker.ps1' file.

4. Start the container: `docker run -it -p 8080:8080 -p 9990:9990 jbossdemocentral/brms-realtime-decicion-server-demo`

5. Follow instructions from above starting at step 5 replacing *localhost* with *&lt;RH_CONTAINER_HOST&gt;* when applicable

Additional information can be found in the jbossdemocentral container [developer repository](https://github.com/jbossdemocentral/docker-developer)


Notes
-----
You will need some sort of Rest client, such as the RESTClient Firefox extension which is used in this demo (screenshots and
videos). After installing RESTClient in Firefox, restart and open it under TOOLS menu.


Supporting Articles
-------------------
- [7 Steps to Your First Rules with JBoss BRMS Starter Kit](http://www.schabell.org/2015/08/7-steps-first-rules-jboss-brms-starter-kit.html)

- [Getting Started with the Realtime Decision Server](http://www.schabell.org/2015/05/jboss-bpmsuite-quick-guide-getting-started-realtime-decision-server.html)


Released versions
-----------------
See the tagged releases for the following versions of the product:

- v1.4 JBoss BRMS 6.4.0.GA on JBoss EAP 7.0.0.GA with demo rule project to deploy as Realtime Decision Server.

- v1.3 JBoss BRMS 6.2.0-BZ-1299002 on JBoss EAP 6.4.4 with demo rule project to deploy as Realtime Decision Server.

- v1.2 JBoss BRMS 6.2.0, JBoss EAP 6.4.4 and demo rule project to deploy as Realtime Decision Server.

- v1.1 JBoss BRMS 6.1 with demo rule project to deploy as Realtime Decision Server and Red Hat Container install option.

- v1.0 JBoss BRMS 6.1 with demo rule project to deploy as Realtime Decision Server


![Digital Sign](./docs/demo-images/digital-sign.jpg)

![Loan Project](./docs/demo-images/loan-prj-overview.png)

![Artifact Repo](./docs/demo-images/artifact-repo-loandemo.png)

![Deployment View](./docs/demo-images/clean-rules-deployment-view.png)

![Kie Server Endpoint](./docs/demo-images/kie-server-endpoint.png)

![Dev Server](./docs/demo-images/dev-server.png)

![Create Container](./docs/demo-images/create-container.png)

![Container Details](./docs/demo-images/container-details.png)

![Start Container](./docs/demo-images/start-container.png)

![Started Container](./docs/demo-images/started-container.png)

![Restapi Auth](./docs/demo-images/restapi-basic-authentication.png)

![Restapi Containers](./docs/demo-images/restapi-containers.png)

![Restapi Loan Container](./docs/demo-images/restapi-container-loan1.0.png)

![Restapi Request Header](./docs/demo-images/restapi-request-header.png)

![Restapi Loan Request](./docs/demo-images/restapi-loan-request.png)

![Restapi Loan Request Response](./docs/demo-images/restapi-loan-request-response.png)
