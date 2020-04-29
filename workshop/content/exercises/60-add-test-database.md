In this exercise you will launch a database ``in a container`` and configure your application to use the database instead of the default built-in database.

First, ensure only one replica of the application is running:

```execute
oc scale dc vote-app --replicas=1
```

# Launch the database in a container

Launch a MySQL database and connect the application to it.  MySQL is a freely available open source Relational Database Management System (RDBMS) that uses Structured Query Language (SQL). 

Run this to start a MySQL container:

```execute
oc new-app --name db mysql:5.7 \
   -e MYSQL_USER=user \
   -e MYSQL_PASSWORD=password \
   -e MYSQL_DATABASE=vote 
```

MySQL will be started and configured with a database called ``vote`` and with ``user`` and `password` credentials, as stated in the above command parameters. 

Take a look at the log output:

```execute
oc logs dc/db
```

It will take about 30 seconds for the database to be running.  You will see `ready for connections` in the log output.  If not, try the above ``oc logs`` command again. 

Once the database is up and running, verify that by checking if the ``vote`` database exists:

```execute
mysql -h db.%project_namespace%.svc -u user -ppassword -D vote -e "show databases"
```

- You should see the following.  If not, wait for the database to come up and/or check the above and try again. 

```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| vote               |
+--------------------+
```

## Connect the application to the database 

First of all, look at the output of the current vote app:

```execute
oc logs dc/vote-app | grep ^Connect 
```

You can see that it's using ``sqlite`` (an internal database) to store it's data.  We will change this below. 

At this point, the application is unaware of the existence of the MySQL database!  You need to re-configure the application to use the new database. 
To do that we will change the vote application's environment variables and re-launch the pod.  The vote application looks for the existence of the environment variables at startup and uses them to configure itself.  If database credentials are defined the application connects to the database and provisions the needed tables and data. 

Connect the application to the database:

```execute
oc set env dc vote-app \
   ENDPOINT_ADDRESS=db \
   PORT=3306 \
   DB_NAME=vote \
   MASTER_USERNAME=user \
   MASTER_PASSWORD=password \
   DB_TYPE=mysql
```

The above command sets the environment variables `as stated in the arguments`. The deployment configuration restarts the pod automatically because of the configuration change.

Check that the application is running properly and was able to connect to the database:

```execute
oc logs dc/vote-app 
```

You should see the following in the output, showing the database credentials used.  If not, wait and try the ``oc logs`` command again.

```
---> Running application from Python script (app.py) ...
Connect to : mysql://user:password@db:3306/vote
...
```

Check that the database is now populated with the vote application tables:

<!--
POD=`oc get pods --selector app=workspace -o jsonpath='{.items[?(@.status.phase=="Running")].metadata.name}'`; echo $POD

kubectl get pods --field-selector=status.phase=Running -o name
-->

```execute
mysql -h db.%project_namespace%.svc -u user -ppassword -D vote -e "show tables"
```

You should see the  ``poll`` and ``options`` tables. 

```
+----------------+
| Tables_in_vote |
+----------------+
| option         |
| poll           |
+----------------+
```


<!--
```
POD=`oc get pods --selector app=workspace -o jsonpath='{.items[?(@.status.phase=="Running")].metadata.name}'`; echo $POD
```
-->

# Test the application 

Post a few random votes to the application using the test script:

```execute 
test-vote-app http://vote-app-%project_namespace%.%cluster_subdomain%
```
(This script can be run multiple times if you wish to make more votes).

To view the results use the following command. You should see the votes:

<!--
```execute 
curl -s http://vote-app-%project_namespace%.%cluster_subdomain%/results.html | grep "data: \["
```
-->

```execute
mysql -h db.%project_namespace%.svc -u user -ppassword -D vote -e 'select * from `option`;'
```

  - ``Note:`` Should the application not work as expected, see below for troubleshooting instructions. 

```
+----+------------------------------+---------+-------+
| id | text                         | poll_id | votes |
+----+------------------------------+---------+-------+
|  1 | Mint                         |       1 |     3 |
|  2 | Fedora                       |       1 |     4 |
|  3 | Debian                       |       1 |     7 |
|  4 | Ubuntu                       |       1 |     3 |
|  5 | Fedora CoreOS                |       1 |     2 |
|  6 | Centos                       |       1 |     5 |
|  7 | Red Hat Universal Base Image |       1 |     7 |
|  8 | Alpine                       |       1 |     1 |
|  9 | Arch                         |       1 |     5 |
| 10 | Other                        |       1 |     3 |
+----+------------------------------+---------+-------+
```

View the containers/pods in the console:

* [View the Pods](%console_url%/k8s/ns/%project_namespace%/pods) 

Open the vote application results page in a browser: 

* [Open the Application](http://vote-app-%project_namespace%.%cluster_subdomain%/results.html) 



Now, the application is no longer dependent on the built-in database and can freely scale out - `add containers` - as needed. 

Go to the console and scale the application pods from 1 to 3 (please do not scale to more than 3).  Change the pod ``count`` via the menu of the Deployment Config called ``vote-app``. 

* [Deployment Configs](%console_url%/k8s/ns/%project_namespace%/deploymentconfigs)

You can also use the following command:

```execute
oc scale dc vote-app --replicas=3
```

After 30-60 seconds, you should see the output in the lower window, similar to this:

```
vote-app-3-52kqp    1/1     Running     0          17s
vote-app-3-nb5fk    1/1     Running     0          17s
vote-app-3-p2j4w    1/1     Running     0          7m45s
```

- Note that all the `state` of the application is stored in the database. Each container/pod is therefore `stateless` and can be freely stopped and started, as needed. 

Check the application is still working as expected.  Data should not be lost: 

[Open the Vote Application](http://vote-app-%project_namespace%.%cluster_subdomain%/results.html) 

 - ``Please remember to scale the vote application back down to 1 or use the following command:``

```execute
oc scale dc vote-app --replicas=1
```

---
That's the end of this exercise.

In this exercise you have launched a database for testing purposes and connected the application to it.  

Now move on to the next exercise. 

---
### Troubleshooting instructions

Is the vote application working? 

When the application starts for the first time and initializes the database the log output should look like this:

![log output 1](images/vote-app-start-1.png)

When the application restarts the log output should look like this:

![log output 1](images/vote-app-start-2.png)

Do you see any error messages in the output? 

If the issue can't be fixed, empty the database and restart the application using the following command: 

```execute 
delete-test-database
```

If the issue still can't be fixed, recreate the database and restart the application using the following command: 

```execute 
recreate-test-database
```

After the script has completed return to the instructions and try again.

<!-- drop table `option`;  delete from `option`;  -->
