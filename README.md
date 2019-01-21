
<div align="center">
    <img src="https://mawrederp.com/assets/landing/images/logo.png" height="128">
    <h2>Frappe Bench</h2>
</div>


The bench is a command-line utility that helps you to install apps, manage multiple sites and update Frappe / ERPNext apps on */nix (CentOS, Debian, Ubuntu, etc) for development and production. Bench will also create nginx and supervisor config files, setup backups and much more.

If you are using on a VPS make sure it has >= 1Gb of RAM or has swap setup properly.

## Installation

### Installation Requirements

You will need a computer/server. Options include:

- A Normal Computer/VPS/Baremetal Server: This is strongly recommended. Frappe/ERPNext installs properly and works well on these
- A Raspberry Pi, SAN Appliance, Network Router, Gaming Console, etc.: Although you may be able to install Frappe/ERPNext on specialized hardware, it is unlikely to work well and will be difficult for us to support. Strongly consider using a normal computer/VPS/baremetal server instead. **We do not support specialized hardware**.

### Operating System

- Linux: Debian, Ubuntu, CentOS are the preferred distros and are well tested. [Arch Linux]
- Mac OS X

### 1. Install Pre-requisites

- Python 2.7 [Python3.5+ also supported, but not recommended for production]
- MariaDB 10+
- Nginx (for production)
- Nodejs
- yarn
- Redis
- cron (crontab is required)
- wkhtmltopdf (version 0.12.5) (for pdf generation)

#### 2. Install Bench

Install bench as a *non root* user,

	git clone https://github.com/frappe/bench bench-repo
	pip install --user -e bench-repo

Note: Please do not remove the bench directory the above commands will create

#### Basic Usage

* Create a new bench

	The init command will create a bench directory with frappe framework
	installed. It will be setup for periodic backups and auto updates once
	a day.

		bench init frappe-bench && cd frappe-bench

* Add a site

	Frappe apps are run by frappe sites and you will have to create at least one
	site. The new-site command allows you to do that.

		bench new-site site1.local

* Add apps

	The get-app command gets remote frappe apps from a remote git repository and installs them. Example: [erpnext](https://github.com/frappe/erpnext)

		bench get-app erpnext https://github.com/frappe/erpnext

* Install apps

	To install an app on your new site, use the bench `install-app` command.

		bench --site site1.local install-app erpnext
		
* Start bench

	To start using the bench, use the `bench start` command

		bench start

	To login to Frappe / ERPNext, open your browser and go to `[your-external-ip]:8000`, probably `localhost:8000`

	The default username is "Administrator" and password is what you set when you created the new site.


---

## Easy Install

- This is an opinionated setup so it is best to setup on a blank server.
- Works on Ubuntu 16.04+, CentOS 7+, Debian 8+
- You may have to install Python 2.7 (eg on Ubuntu 16.04+) by running `apt-get install python-minimal`
- You may also have to install build-essential and python-setuptools by running `apt-get install build-essential python-setuptools`
- This script will install the pre-requisites, install bench and setup an ERPNext site
- Passwords for Frappe Administrator and MariaDB (root) will be asked
- MariaDB (root) password may be `password` on a fresh server
- You can then login as **Administrator** with the Administrator password
- If you find any problems, post them on the forum: [https://discuss.erpnext.com](https://discuss.erpnext.com)

Open your Terminal and enter:

#### 1. Download the install script

For Linux:

	wget https://raw.githubusercontent.com/frappe/bench/master/playbooks/install.py


#### 2. Run the install script

If you are on a fresh server and logged in as root, use --user flag to create a user and install using that user

	python install.py --develop --user frappe

For developer setup:

	sudo python install.py --develop

For production:

	sudo python install.py --production --user frappe

#### What will this script do?

- Install all the pre-requisites
- Install the command line `bench`
- Create a new bench (a folder that will contain your entire frappe/erpnext setup)
- Create a new ERPNext site on the bench 

#### How do I start ERPNext

1. For development: Go to your bench folder (`frappe-bench` by default) and start the bench with `bench start`
2. For production: Your process will be setup and managed by `nginx` and `supervisor`. [Setup Production](https://frappe.io/docs/user/en/bench/guides/setup-production.html)

---

Help
====

For bench help, you can type

	bench --help
