# ElasticSearch and Kibana Instalation with Ansible

This Ansible playbook sets up a multi-node Elasticsearch. The playbook assumes Rhell 8 as the operating system.

## As long as you don't specify the version this playbook will install latest version of 8.x.x of Elasticserach.

You can try to install a specif version, by changing to older repository url on the playbook.

## Requirements
- Ansible 2.9.6 or higher
- Access to ELK servers, from Ansible-Node
- SSH access to each server with sudo privileges

### Usage

1. Clone this repository to your local machine:

		git clone https://github.com/tryingtobecoder34/es-ansible-installation-centos

2. Modify the inventory & Jinja2 files for you enviroment, include the hostnames or IP addresses of your three servers.

3. Modify the elasticsearch.yml.j2 file to set the cluster name, JVM heap size, network host, and xpack security settings as desired.

4. Run the Ansible-Playbook, be sure your user have permission to make changes on the remote hosts, or you can add -u root, parameter to auth as a root user to other hosts
	
		ansible-playbook -i inventory.ini playbook.yml -u root
		ansible-playbook -i inventory.ini renew-certificate.yml -u root


5. You need to mannually run these commands, and copy the password when the promp on the screen, after generating the password go to Kibana configuration file, which is in /etc/kibana, then re-write the password for kibana_system user.

	Create elasticsearch password (passwords must be same with certificate passwords.), on es01, es02, es03.
			
		/usr/share/elasticsearch/bin/elasticsearch-keystore create -p

	Generate passwords for built-in users on es01,
			
		/usr/share/elasticsearch/bin/elasticsearch-setup-passwords auto


### Configuration

The variables can be set in the elasticsearch.yml.j2 file to customize the Elasticsearch installation.


- Check the cluster health with below command.
		
		curl -XGET http://es02:9200/_cluster/health?pretty=true
		curl -XGET http://es02:9200/_cat/nodes?pretty=true


- The output should look like this for 3 node cluster, which has 2 datanodes.


	https://i.ibb.co/pvSfpJT/Screenshot-at-Mar-01-18-29-12.png



To uninstall the elastic search some helpfull commands.

	sudo yum remove elasticsearch
	sudo yum remove kibana
	sudo rm /var/lib/elasticsearch -rf
	sudo rm /var/lib/kibana -rf

