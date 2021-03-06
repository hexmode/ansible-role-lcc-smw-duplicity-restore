lcc-smw-duplicity-restore
=========================

This role restores a [Semantic MediaWiki](http://www.semantic-mediawiki.org) instance using [Duplicity](http://duplicity.nongnu.org/) by executing the following steps:

1. Create the dedicated OS user (matching the original DB user) for executing the SMW restore process
2. Create the dedicated SMW root to copy the restored SMW root content into
3. Create the dedicated database to import the SMW database backup .sql into
4. Use [Duplicity](http://duplicity.nongnu.org/) to restore a backup set
5. Copy the restored SMW root content into the dedicated SMW root created in step 2
6. Import the restored SMW database backup .sql into the database created in step 3
7. PENDING: Run the following SMW maintenance scripts:
	- maintenance/runJobs.php
	- maintenance/rebuildall.php
	- extensions/SemanticMediaWiki/maintenance/rebuildData.php
	- extensions/Cargo/maintenance/cargoRecreateData.php (if installed)

Best Practice Pattern: How to repeat 'vagrant up' while keeping a previously restored VM in VirtualBox
------------------------------------------------------------------------------------------------------

1. In VirtualBox select the previously restored VM
2. Select in menu Machine > Clone...
3. Add a timestamp as a prefix to the VM's name
4. Check to "Reinitialize the MAC address of all network cards"
5. Click "Continue"
6. Select "Full clone"
7. Click "Clone"
8. You may now do `vagrant destroy && vagrant up`, which will create another restore

Requirements
------------

* [Duplicity](http://duplicity.nongnu.org/)
* [GnuPG](https://www.gnupg.org/)

Role Variables
--------------

1. Copy the file defaults/variables.yml next to your playbook and fill in accordingly.
2. Encrypt variables.yml file executing `ansible-vault encrypt variables.yml` and set a strong password.

If you use this playbook to restore an SMW into a Vagrant box (e.g. [ch.l-c-c.standard-smw](https://atlas.hashicorp.com/LinuxCompetenceCenter/boxes/ch.l-c-c.standard-smw)), then include this file in your Vagrantfile by `ansible.extra_vars = "variables.yml"`.

You will be prompted for the password each time you execute the playbook (whether within a Vagrantfile or otherwise).

Dependencies
------------

This role doesn't depend on other roles hosted on Ansible Galaxy.

Example Playbook
----------------

	---

	- hosts: all
	  sudo: True
	  gather_facts: False
	  roles:
	    - LinuxCompetenceCenterGalaxy.lcc-smw-duplicity-restore
	  post_tasks:
	  	Place special operations here. (e.g. for adapting LocalSettings.php)

License
-------

GNU GENERAL PUBLIC LICENSE

Author Information
------------------

lex@l-c-c.ch