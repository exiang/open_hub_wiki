## Neo4J Installation Guide

Add the Neo4J key into the apt package manager:
```bash
wget -O - http://debian.neo4j.org/neotechnology.gpg.key | apt-key add -
```

Add Neo4J to the Apt sources list:

```bash
echo 'deb http://debian.neo4j.org/repo stable/' > /etc/apt/sources.list.d/neo4j.list
 -O - http://debian.neo4j.org/neotechnology.gpg.key | apt-key add -
```

Update the package manager:

```bash
sudo apt-get update
```

Install Neo4J:

```bash
sudo apt-get install neo4j
```

Neo4J should be running. You can check this with the following command


```bash
sudo service neo4j status
```

## For more information 
Please refer [DigitalOcean Neo4J Installation](https://www.digitalocean.com/community/tutorials/how-to-install-neo4j-on-an-ubuntu-vps) guide.