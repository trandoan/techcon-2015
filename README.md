# techcon-2015
Demo boxes for simulate environment for Techcon 2015

Just run

```bash
vagrant up
```

By default, 3 boxes will be created by Vagrant:
+ `ci-server` - Contains CI server (drone.io) and some base packages: `docker`
+ `dev` - Contains base packages: `docker`, `docker-compose`
+ `prod` - Contains base packages: `docker`, `docker-compose`