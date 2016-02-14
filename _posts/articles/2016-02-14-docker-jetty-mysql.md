---
layout: post
title: "Minimal Docker Jetty - Mysql application"
excerpt: "What does it take to setup a minimal app to explain two containers working with each other?"
categories: articles
tags: [technology, linux, jetty, mysql, docker]
author: sudharsan
comments: true
share: true
image:
  feature: so-simple-sample-image-2.jpg
---

I personally find it much easier by looking at a working system, as opposed to getting a new system up-and-running. Probably a hang-over from my childhood days dismantling things...

In this article, I am sharing the quickest way to get a small jsp page connect to mysql instance, both running on docker containers. 

* TOC
{:toc}

Assumptions
=================
For purposes of this article, I am assuming:

1. To avoid any confusion, I recommend that you should NOT have mysql (server/client) or jetty on the local system. And local ports 8080, 443, 3306 are available for use.
1. Java, ant build system is available
1. Docker engine is available. In case of Ubuntu, you can [find more information here](https://docs.docker.com/engine/installation/linux/ubuntulinux/).
1. git is installed
1. You understand that the code is meant to be simple (clear-text passwords, poor error-handling). Do not use it for anything beyond learning ...


Get the code
=================

Pull the latest bare minimum code (jars, Dockerfiles, jsp) from github

{% highlight shell %}
git clone https://github.com/p2c2e/min-docker-jetty-mysql.git
{% endhighlight %}

{% highlight shell %}
cd min-docker-jetty-mysql
{% endhighlight %}

Ensure there is no other local instance of mysql (port 3306 should be free)
Note that there is no data folder at this point

Setting up the MySQL server container instance
=================

{% highlight shell %}
docker pull mysql
./bin/dmysqlserver.sh 
{% endhighlight %}

Check if the server is up and running 

{% highlight shell %}
docker ps -a | grep mysql-server
docker logs mysql-server
{% endhighlight %}

Look for "mysqld : ready for connections"

At this point, not only is the mysql-server up, it should also have a new database called 'TestDB'. You can verify that by running below commands:

{% highlight shell %}
./bin/dmysql.sh 
{% endhighlight %}

In the MySQL client prompt, run: 
{% highlight shell %}
show databases;
{% endhighlight %}
The TestDB database was created for you because there is a 'init' sql file in the ./startup folder. The mysql-server container executes all the files in that folder as part of initialization.

Setting up the jetty side
=======================

{% highlight shell %}
docker pull jetty
{% endhighlight %}

Now launch jetty using the shell/batch script

{% highlight shell %}
./bin/djetty.sh
{% endhighlight %}

Verify jetty is up by checking the following:

{% highlight shell %}
docker ps -a | grep jetty-server
{% endhighlight %}

Verify if the site is up and running by visiting [the Jetty Root](http://localhost:8080/) 

Since there is no ROOT.war distributed, there should be a context listing page with no apps deployed. Jetty is checking for webapps in the ./webapps folder

Deploy an app and test jetty / MySQL connectivity
=======================

Now deploy a bare-minimum webpp consisting of a index.jsp connecting to a DB and dropping/creating/listing the table row count.

{% highlight shell %}
ant 
{% endhighlight %}

This should create and copy over a 'war' file to the ./webapps/ folder

Check if the war was successfully deployed using :

{% highlight shell %}
docker logs jetty-server
{% endhighlight %}

Now visit: [http://localhost:8080/java-mysql-1.0/](http://localhost:8080/java-mysql-1.0/)

It should display a page like below:

![Minimal Jetty MySQL Docker Homepage](/images/simple jetty page.png)

How does the Jetty server container access the MySQL server container?
=======================

Legacy Links - 

1. The primary way by which containers expose their services is through ports. The docker engine can expose pre-defined ports automaticaly or we can do that explicitly using the -p to:from options
1. For one container to talk to another, the simplest way is to a) expose the mysqld port to the outside world and b) share the mysql servers IP to the jetty-server. This is done via --link option (see the ./bin/djetty.sh)

Though the wiring of the two containers were done in the shell scripts, we can 'compose' the two containers together so that the dependencies are clearer. 

Accomplishing the container wiring through 'docker-compose'
=======================

First verify that docker-compose is installed properly. Use [this link](https://docs.docker.com/compose/install/) to get you set-up.

Let us kill the existing containers before we can try out the docker-compose version of the application.

{% highlight shell %}
docker kill jetty-server mysql-server 
docker rm $(docker ps -a -q -f status=exited)
{% endhighlight %}

Ensure you are in the root of the clone repository (root directory should contain the docker-compose.yml file). Now, run the docker-compose version of the application using:

{% highlight shell %}
docker-compose up 
{% endhighlight %}
If you used the non-daemon version, you will clearly see when the system is ready to accept requests. You can shutdown the servers by Ctrl-D.

Alternatively (if you want to run everything as a daemon)...
{% highlight shell %}
docker-compose up -d 
{% endhighlight %}
In this case, you would need to shutdown the servers using 

{% highlight shell %}
docker-compose down
{% endhighlight %}

After about 20 seconds, you will be able to visit [http://localhost:8080/java-mysql-1.0/](http://localhost:8080/java-mysql-1.0/).

Newbie tips
=======================

1. When building images using 'docker build', there are many occasions where there are intermediate images that might be left behind. Use 'docker rmi <imageid>' to remove them
1. When you build and tag a new image - if there is an image already exists with that name, it gets untagged. Importantly, the image still remains in the cache. You will need to manually remove them
1. Containers that have exited (normally or due to getting killed), continue to remain on disk. If not cleaned up, they continue to take up significant space. Remove them using 'docker rm <containerid>'
1. alias the following command to make things easier. The command removes any containers that are lingering after they have exited: 
{% highlight shell %}
docker rm $(docker ps -a -q -f status=exited)
{% endhighlight %}
1. You can copy-over/run the d*.sh scripts in the repo and use it for generically  
	If you run the dmysqlserver.sh from any directory, it initializes a brand new instance of mysql and stores the data in the ./data folder. It also creates a 'startup' folder where we can store any bootstrap / db-dumps to initialize the database
	Using the djetty.sh from any directory, it creates / uses the webapps/ subfolder to deploy apps
	Due to port and container name conflicts, you can only run one instance of jetty, mysql-server at any one time



