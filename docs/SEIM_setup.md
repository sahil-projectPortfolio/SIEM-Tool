
# SEIM- ELK Stack

The Elastic Stack — formerly known as the ELK Stack — is a collection of open-source software produced by Elastic which allows you to search, analyze, and visualize logs generated from any source in any format, a practice known as centralized logging. 

The Elastic Stack has four main components:

- Elasticsearch: A distributed RESTful search engine which stores all of the collected data.
- Logstash: The data processing component of the Elastic Stack which sends incoming data to Elasticsearch.
- Kibana: A web interface for searching and visualizing logs.
- Beats: Lightweight, single-purpose data shippers that can send data from hundreds or thousands of machines to either Logstash or Elasticsearch.

# 1. Configuration of Virtual Machine
Go to edit virtual machine settings. Then, select network adapter to a bridged network. 
A bridged network connects the virtual machine to a physical network adapter in the host system. It allows the virtual machine to connect to the local area network(LAN) that the host machine uses. Also, the bridged network creates a unique identity on the network for the virtual machine. This unique identity separates the virtual machine from the host system. 
This allows the virtual machine to access other machines on the network and contact other machines as it is a physical computer on the network.

# 2. Installing and Configuring Elasticsearch  

Please run the below commands with sudo permissions:

    curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch |sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg
    echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
    sudo apt update
    sudo apt-get install elasticsearch
    sudo nano /etc/elasticsearch/elasticsearch.yml
    Change the network.host to your VM’s IP address
    sudo systemctl start elasticsearch
    sudo systemctl enable elasticsearch
    curl -X GET "localhost:9200"

    Troubleshooting #1: If you get the output, curl: (52) Empty reply from server.

    > Stop the elasticsearch process using the command: sudo systemctl stop elasticsearch.
    > Go back and edit the elasticsearch file using the command: sudo nano /etc/elasticsearch/elasticsearch.yml.
    Scroll until you find Begin Security Auto Configuration. Under the enable security features, change xpack.security.enabled from true to false.
    > Start the elasticsearch process using the command: sudo systemctl start elasticsearch.
    > Run the command: curl -X GET “localhost:9200” again and you should get a response. The response indicates that you can connect to the Elasticsearch server.

# 3. Installing and Configuring the Kibana Dashboard  

According to the official documentation, you should install Kibana only after installing Elasticsearch. Installing in this order ensures that the components each product depends on are correctly in place.

    sudo apt install kibana
    sudo systemctl enable kibana
    sudo systemctl start kibana
    sudo service kibana stop.
    sudo nano /etc/kibana/kibana.yml. 
    Under the System section, uncomment the elasticsearch.host and change the IP address to your VM’s IP address.
    sudo service kibana start

# 4. Installing Auditbeat

    sudo apt-get install auditbeat -y
    sudo systemctl start auditbeat
    sudo nano /etc/auditbeat/auditbeat.yml. 
    Under the Elasticsearch Output, uncomment the hosts and change the IP address to your VM’s IP address
    sudo auditbeat -e setup
    This command connects to Kibana and uploads the templates to the Kibana dashboard. Type into the browser: http://localhost:5601
     
# Explore
Visit Discover, you can look around and see what data your SIEM is collecting.
Explore the Dashboard section and visualise the data accordingly.