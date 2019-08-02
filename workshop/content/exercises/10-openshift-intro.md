In this lab you will access OpenShift and familiarize yourself with the command line and also the web console. 

View how you are authenticated with OpenShift:

```execute
oc whoami
```

This is your username. 

View the version of both oc - the client - and also the server:

```execute
oc version
```

Note the version of the server. Look for the "Major" and "Minor" numbers, e.g. 4 and 1+.

View the server endpoint through which you are authenticated.

```execute
oc whoami --show-server
```

View which projects (namespaces) you have access to:

```execute
oc projects
```

Fetch the console URL:

```execute
oc whoami --show-console
```

Have a look at the OpenShift Console and view the following:

Your projects can be viewed here:

* [Projects](%console_url%) 

The status of your project can be seen here:

* [Status](%console_url%/overview/ns/%project_namespace%)

OpenShift events can be viewed here:

* [Events](%console_url%/k8s/ns/%project_namespace%/events)

If something is not working properly, this is a good place to look.

Various OpenShift objects can be viewed:

* [Pods](%console_url%/k8s/ns/%project_namespace%/pods) 
* [Builds](%console_url%/k8s/ns/%project_namespace%/buildconfigs)
* [Deployments](%console_url%/k8s/ns/%project_namespace%/deploymentconfigs)
* [Routes](%console_url%/k8s/ns/%project_namespace%/routes) 
* [Workloads](%console_url%/k8s/cluster/projects/%project_namespace%/workloads)

You can view various technologies, including source to image, Templates and Operators here:

* [Developer Catalog](%console_url%/catalog/ns/%project_namespace%)

<!--
FIXME: is this useful?:
* [Operator management](%console_url%/operatormanagement/ns/%project_namespace%)
-->

Come back to the terminal tab or click here:

* [Terminal](%terminal_url%)

Note that you can open the OpenShift Console in a separate tab by using the menu on the top right corner.

---
That's the end of this lab.

In this lab you were introduced to the OpenShift console and command line.  In the next lab, you will load and run your first container based application. 


