

#                       1 er 

  #########
  # Wazuh #
  #########

In /opt/OSSIEM/wazuh

# 1- Increase max_map_count on your host (Linux). This command must be run with root permissions:
sysctl -w vm.max_map_count=262144

# 2- Run the certificate creation script:
sudo docker compose -f generate-indexer-certs.yml run --rm generator

# Copy the root-ca.pem
cd /opt/OSSIEM/wazuh/config/wazuh_indexer_ssl_certs/
cp root-ca.pem /opt/OSSIEM/graylog/

# Vérifier qui'il soit bien là 
cd /opt/OSSIEM/graylog/
ls

  ###########
  # Graylog #
  ###########

sudo chown 1100:1100 *
ls -llhh

# 3- Start the environment with docker-compose:

docker compose up -d










You can now safely start all containers by running:

docker compose up -d


###########
# Graylog #
###########

# Ajout du certificat

docker exec -it graylog bash

cp /opt/java/openjdk/lib/security/cacerts /usr/share/graylog/data/config/

cd /usr/share/graylog/data/config/

ls 

keytool -importcert -keystore cacerts -storepass changeit -alias wazuh_root_ca -file root-ca.pem
        yes

exit

sudo docker restart graylog

sudo docker logs graylog


#########
# Wazuh #
#########

# Wazuh Rules

docker exec -it wazuh.manager /bin/bash
dnf install git -y
curl -so ~/wazuh_socfortress_rules.sh https://raw.githubusercontent.com/socfortress/OSSIEM/main/wazuh_socfortress_rules.sh && bash ~/wazuh_socfortress_rules.sh
        yes
cd /var/ossec/etc/rules/
ls
/var/ossec/bin/wazuh-control status


  ################
  # Velociraptor #
  ################
We now need to generate the api.config.yaml file which CoPilot will use to access the Velociraptor API

docker exec -it velociraptor /bin/bash
ls
./velociraptor --config server.config.yaml config api_client --name admin --role administrator,api api.config.yaml
# Il devrait y avoir un fichier désormais api.config.yaml
ls 
cat api.config.yaml

# Accès à l'api velociraptor