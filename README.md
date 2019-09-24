# PHP source to image Helloworld Example

This is an example php application, which can be deployed to APPUiO using the following commands

## How to deploy

### Webconsole

* "Add to Project" a php:5.6 application
* name the application for example appuio-php-sti-example and provide the git repository URL, in this example https://github.com/rloisell/example-php-sti-helloworld.git
* the build and deployment is automatically triggered and the example application will be deployed soon

### CLI / oc Client

#### Create New OpenShift Project - not required if using AG LAB Space on BC Gov OCP
```
$ oc new-project example-php-sti-helloworld
```

#### Create Application and expose Service
```
$ oc new-app https://github.com/rloisell/example-php-sti-helloworld.git --name=agpssg-php-sti-example
```
#### OUTPUT:

$ oc new-app https://github.com/rloisell/example-php-sti-helloworld.git --name=agpssg-php-sti-example
--> Found image e71521d (8 months old) in image stream "openshift/php" under tag "7.1" for "php"

    Apache 2.4 with PHP 7.1 
    ----------------------- 
    PHP 7.1 available as container is a base platform for building and running various PHP 7.1 applications and frameworks. PHP is an HTML-embedded scripting language. PHP attempts to make it easy for developers to write dynamically generated web pages. PHP also offers built-in database integration for several commercial and non-commercial database management systems, so writing a database-enabled webpage with PHP is fairly simple. The most common use of PHP coding is probably as a replacement for CGI scripts.

    Tags: builder, php, php71, rh-php71

    * The source repository appears to match: php
    * A source build using source code from https://github.com/rloisell/example-php-sti-helloworld.git will be created
      * The resulting image will be pushed to image stream tag "agpssg-php-sti-example:latest"
      * Use 'start-build' to trigger a new build
    * This image will be deployed in deployment config "agpssg-php-sti-example"
    * Ports 8080/tcp, 8443/tcp will be load balanced by service "agpssg-php-sti-example"
      * Other containers can access this service through the hostname "agpssg-php-sti-example"

--> Creating resources ...
    imagestream.image.openshift.io "agpssg-php-sti-example" created
    buildconfig.build.openshift.io "agpssg-php-sti-example" created
    deploymentconfig.apps.openshift.io "agpssg-php-sti-example" created
    service "agpssg-php-sti-example" created
--> Success
    Build scheduled, use 'oc logs -f bc/agpssg-php-sti-example' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose svc/agpssg-php-sti-example' 
    Run 'oc status' to view your app.
$ 

```
$ oc expose service agpssg-php-sti-example
```

OUTPUT:
$ oc expose service agpssg-php-sti-example
route.route.openshift.io/agpssg-php-sti-example exposed
$

#### Get URL
```
$ oc status | grep php
```
OUTPUT:
$ oc status | grep php
http://agpssg-php-sti-example-ag-devops-lab-deploy.pathfinder.gov.bc.ca to pod port 8080-tcp (svc/agpssg-php-sti-example)
  dc/agpssg-php-sti-example deploys istag/agpssg-php-sti-example:latest <-
    bc/agpssg-php-sti-example source builds https://github.com/rloisell/example-php-sti-helloworld.git on openshift/php:7.1 
$

## Add Webhook to trigger rebuilds

Take the Webhook GitHub URL from

```
$ oc describe bc agpssg-php-sti-example
Name:		agpssg-php-sti-example
Namespace:	ag-devops-lab-deploy
Created:	7 minutes ago
Labels:		app=agpssg-php-sti-example
Annotations:	openshift.io/generated-by=OpenShiftNewApp
Latest Version:	1

Strategy:	Source
URL:		https://github.com/rloisell/example-php-sti-helloworld.git
From Image:	ImageStreamTag openshift/php:7.1
Output to:	ImageStreamTag agpssg-php-sti-example:latest

Build Run Policy:	Serial
Triggered by:		Config, ImageChange
Webhook Generic:
	URL:		https://console.pathfinder.gov.bc.ca:8443/apis/build.openshift.io/v1/namespaces/ag-devops-lab-deploy/buildconfigs/agpssg-php-sti-example/webhooks/<secret>/generic
	AllowEnv:	false
Webhook GitHub:
	URL:	https://console.pathfinder.gov.bc.ca:8443/apis/build.openshift.io/v1/namespaces/ag-devops-lab-deploy/buildconfigs/agpssg-php-sti-example/webhooks/<secret>/github
Builds History Limit:
	Successful:	5
	Failed:		5

Build				Status		Duration	Creation Time
agpssg-php-sti-example-1 	complete 	1m36s 		2019-09-23 22:05:36 -0700 PDT

Events:	<none>
$
```

and add the URL as a Webhook in your github Repository, read https://developer.github.com/webhooks/ for more details about github Webhooks
