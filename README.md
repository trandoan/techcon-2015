# techcon-2015

###0. Notes:

We will create a local universe:

+ CI Server:
    + JDK 7
    + Maven 3
    + Jenkins:
        + Maven Project Plugin
        + Environment Injector Plugin
        + CloudBees Docker Build and Publish Plugin
        + Git Plugin
        + Publish Over SSH Plugin
    + Stash (Git repository, free for personal usage, integrate well with Jenkins)
    + Registry (Docker hub)
    + Docker & docker-compose
+ Dev Server:
    + Docker & docker-compose
+ Prod Server:
    + Docker & docker-compose

###1. Create demo boxes
Demo boxes for Techcon 2015
Navigate to clone repository, just run

```bash
vagrant up
```

By default, 3 boxes will be created by Vagrant:
+ `ci-server` - Contains base packages: `docker`, `docker-compose`
+ `dev` - Contains base packages: `docker`, `docker-compose`
+ `prod` - Contains base packages: `docker`, `docker-compose`

Get VM's IP

```bash
vagrant ssh <VM> # E.g. ci, dev, prod
ifconfig
```

**Root login** is disable.
Credentials: `vagrant/vagrant`

###2. Install 3rd-party applications

Install JDK7 and maven normally.

[Install Jenkins](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu) - `ci-server:8080`
Install registry: - `ci-server:5000/v1/search` In clone repo, execute command:

```bash
docker-compose -f 3rd-party up -d --no-recreate registry
```

[Install stash](https://registry.hub.docker.com/u/atlassian/stash/): - `ci-server:7990`
```bash
docker run -u root -v /data/stash:/var/atlassian/application-data/stash atlassian/stash chown -R daemon  /var/atlassian/application-data/stash
docker run -v /data/stash:/var/atlassian/application-data/stash --name="stash" -d -p 7990:7990 -p 7999:7999 atlassian/stash
```

To restart Stash:
```bash
docker stop stash
docker start stash
```

###3. Set up build job in Jenkins
Demo applications:
+ [App A](https://github.com/trandoan/appA)
+ [App B](https://github.com/trandoan/appB)
+ [App C](https://github.com/trandoan/appC)
+ [Deployment](https://github.com/trandoan/deployment)

For each application, we will create 2 build jobs: Development & Production (Except `Deployment`, of course)

![Imgur](http://i.imgur.com/QTsAmHS.png)

Build job configuration sample:

![Imgur](http://i.imgur.com/fs1T3Xk.png)
![Imgur](http://i.imgur.com/hK3YIXI.png)

> `Poll SCM` must be enable due to Stash integration

![Imgur](http://i.imgur.com/SZZTOg9.png)
![Imgur](http://i.imgur.com/Cyof4Wq.png)
![Imgur](http://i.imgur.com/MOTuDz8.png)
![Imgur](http://i.imgur.com/iQaFgfW.png)

```bash
cd $DEPLOY_PATH &&
sed -i -e 's#.*techcon/app-a.*#  image: $PRIVATE_REGISTRY/techcon/app-a:$POM_VERSION#g' -e 's#.*techcon/app-b.*#  image: $PRIVATE_REGISTRY/techcon/app-b:$APP_B_VERSION#g' -e 's#.*techcon/app-c.*#  image: $PRIVATE_REGISTRY/techcon/app-c:$APP_C_VERSION#g' docker-compose.yml &&
docker-compose pull --allow-insecure-ssl &&
docker-compose up -d --allow-insecure-ssl --x-smart-recreate
```

###4. Configure Development and Production Server:
Demo applications:
+ [App A](https://github.com/trandoan/appA)
+ [App B](https://github.com/trandoan/appB)
+ [App C](https://github.com/trandoan/appC)
+ [Deployment](https://github.com/trandoan/deployment)

> Make sure you can build `App A`, `App B`, `App C` successfully and push the images to local registry

Clone [Deployment](https://github.com/trandoan/deployment) to somewhere. Remember that path (used to configure build job in Jenkins)



