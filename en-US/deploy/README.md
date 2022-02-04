---
root: true
name: Deployment
sort: 7
---

# Releasing and Deploying

### Development mode

The application created by `radical` is in development mode by default.

We can change the mode by:

	radiant.RunMode = "prod"

Or change it in conf/app.conf:

	runmode = prod


In development mode:

- If you don't have a views folder, it will show this kind of error:

		2013/04/13 19:36:17 [W] [stat views: no such file or directory]

- Templates will load every time without cache.

- If server throws error, the response will look like:

![](./../images/dev.png)

### Releasing and Deploying

The Go application is a bytecode file after compiling. You just need to copy this file to the server and run it. But remember Radiant might also include static files, configuration files and templates, so these three folders also need to be copied to server while deploying.

	$ mkdir /opt/app/radicalpkg
	$ cp radicalpkg /opt/app/radicalpkg
	$ cp -fr views /opt/app/radicalpkg
	$ cp -fr static /opt/app/radicalpkg
	$ cp -fr conf /opt/app/radicalpkg

Here is the folder structure in `/opt/app/radicalpkg`:

	.
	├── conf
	│   ├── app.conf
	├── static
	│   ├── css
	│   ├── img
	│   └── js
	└── views
	    └── index.tpl
	├── radicalpkg

Now we've copied our entire application to the server. Next step is deploy it.

There are three ways to run it:

- [Stand alone deploy](./radiant.md)
- [Deploy with Supervisord ](./supervisor.md)
- [Deploy with Systemctl ](./systemctl.md)
	
The application is exposed above, then usually we will have a nginx or apache to serve pages and perform load balancing on our application.

- [Deploy with Nginx](./nginx.md)
- [Deploy with Apache](./apache.md)
