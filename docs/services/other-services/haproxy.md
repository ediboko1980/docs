# Introduction

Supported versions:

`latest`

You might find helpful our previous articles about configuring HAProxy ([Part one](https://d2c.io/post/haproxy-load-balancer-part-1-basic-terms-levels) and [Part Two](https://d2c.io/post/haproxy-load-balancer-part-2-backend-section-algorithms)).

## Creating HAProxy service

At first, you need to open or create any project and click **New service**. You can also use **Create** button in the menu and choose **Service**. You will see a list of services which can be deployed with D2C:

![Create service](../../img/new_interface/create_service.png)

Choose HAProxy

## General settings

![Create haproxy service - general settings](../../img/new_interface/creating_service_haproxy_general_settings.png)

### Name

The name should start with a letter and contain up to 16 characters (Latin letters, numbers, hyphen).

Each service has its unique name. Services can communicate with each other by service names (or alias-names, e.g. `servicename-1` or `servicename`).

### Version

You can choose a version for your application from a drop down list.

## Network settings

![Create haproxy service - network settings](../../img/new_interface/creating_service_nginx_network_settings.png)

By default, application containers are started inside a private network and have dynamically assigned local IP addresses. Apps can reference each other by service name. It does not matter on which host the app is running – all private network intercommunication is transparent for all services in your account.

Access from the Internet is enabled for NGINX by default. You can disable access from the Internet while creating or editing service.

## Extra settings

![Create haproxy service - extra settings](../../img/new_interface/creating_service_db_extra_settings.png)

### Initial commands

Commands which are executed only once on the first container after the first deploying a service. You can use it for populate a database or migration, run an install of WordPress, etc.

### Commands for installing global dependencies

Commands for installing global dependencies and modifying Docker image of your service. For modifying source code use Local dependencies.

Examples: `pip install`, `bundle install`, `apt-get install`, `npm install -g`

### Local dependencies and code’s preparation

Commands for installing local dependencies and making your code ready to work.

Examples: `npm install`, `composer install`, `bower install`, etc. or do some for preparation: `gulp build`, `grunt build`, etc.

## Environment variables

![Create service - environment variables](../../img/new_interface/creating_service_db_env.png)

You can specify environment variables for your application. They can be edited after creating a service.

## Persistent data volumes

![Create service - persistent data volumes](../../img/new_interface/creating_service_haproxy_volumes.png)

D2C separates the application itself from its data. Docker volumes are used to store persistent data. Data is stored locally on the hosts. Any data which is generated by an application should be added to Persistent data volumes. **All modifications outside of these directories will be deleted after restart/rebuild/redeploy of a container/service (Docker restores the original state of a container)**.

You can create new volumes using **Add volume** button.
The host path by default is created automatically. You can change it but be careful. **It can break the whole host**.

### Synchronizing

We recommend storing _user generated content_ in cloud storage like [Amazon S3](https://aws.amazon.com/s3/) or CDN. Sometimes there is a need to support old technologies when data stores at hosts. For such cases you can use synchronization volumes between all containers. Simply check the volumes which you want to sync when creating or editing service. D2C uses Lsync for synchronization.

## Configure services

![Create haproxy - configure services](../../img/new_interface/creating_service_nginx_configure_services.png)

The actions and behaviour are almost the same as [configuring NGINX](/services/other-services/nginx/). The difference is that HAProxy can serve only one service when NGINX can serve any amount of services.

You can choose what service HAProxy serves by clicking on **Add service config** button.

![Create haproxy - configure services choose project](../../img/new_interface/creating_service_nginx_configure_services_current.png)
 or
![Create haproxy - configure services choose project from different project](../../img/new_interface/creating_service_nginx_configure_services_diff_project.png)

After you choose a service you can see a set of settings:

![Create haproxy - configure services choose mode](../../img/new_interface/creating_service_haproxy_mode.png)

### Mode

- HTTP
- HTTPS
- TCP

### Config. HTTP

![Create haproxy - configure services http](../../img/new_interface/creating_service_haproxy_http_config.png)

Clicking on **Generate config** button, you see an automatically generated config for the selected service.

Click on **Edit** button if you need to change your config. You can back to defaults using **reset to default** button.

### Domains

![Create haproxy - configure services domains](../../img/new_interface/creating_service_haproxy_domains.png)

By default it has a http config and automatically generated domain (**[servicename]-[www].[accountID].[at].d2c.io**).

You can set your own domains clicking on **Use your own domains** link or just removing D2C domain and paste yours.

!!! DNS-record

    DNS-record for your domains with the same IP-address as a host of NGINX should be accessible at the moment of creating or editing service

### Sticky sessions

Reduce the number of connections, and this is one of the ways to allow WebSockets work on multiple hosts. When you turn it on a user connects to the same host all the time.

### Config. HTTPS with Let's Encrypt

![Create haproxy - configure services let's encrypt](../../img/new_interface/creating_service_haproxy_lets_encrypt.png)

!!! Limitations

    Let's Encrypt has [rate limits](https://letsencrypt.org/docs/rate-limits/) and [may not be trusted by old browsers](https://community.letsencrypt.org/t/which-browsers-and-operating-systems-support-lets-encrypt/4394)

#### Renewal

All Let’s Encrypt certificates issued using D2C renew automatically. The request for renewal is sent every day.

#### Security Level

There are two security levels available:

- Medium (TLS 1, 1.1, 1.2)
- High (TLS 1.2)

!!! Requirements

    Minimum requirements for High security (TLS 1.2): Firefox 27, Chrome 30, IE 11 on Windows 7, Edge, Opera 17, Safari 9, Android 5.0, and Java 8

## Configs

![Create service - configs](../../img/new_interface/creating_service_haproxy_configs.png)

You can add your additional config files. These files **do not change after restart/rebuild/redeploy** of your applications.

Click on the “reset to default” button if you need to return to default config.

## Hosts

![Create service - hosts](../../img/new_interface/creating_service_single_host.png)

You can choose one or several hosts where the similar containers will be deployed. It’s not necessary at the start, and you can scale your services after deployment. Also, at this step you can create new hosts and choose them even they are not online yet (creating status). The containers will be deployed when they are ready.

### Busy ports

![Create service - busy hosts](../../img/new_interface/creating_service_nginx_busy_port.png)

HAProxy has enabled access from the Internet by default. 80 and 443 ports should be free at a host which you want to use for deploying NGINX. If D2C manages a host from scratch (using supported cloud providers, not the own one) the busy ports are shown on a host card.
