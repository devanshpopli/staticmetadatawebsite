### Instructions to host static website on an Azure Virtual Machine.

1. Install Apache web server on vm
    sudo apt-get update
    sudo apt-get install apache2
2. Install jq package to manage json objects
    sudo apt-get install jq
3. Get meta data of vm using curl command and store in json file
    sudo bash -c "curl -s -H Metadata:true --noproxy "*" "http://169.254.169.254/metadata/instance?api-version=2021-02-01" | jq '.' > file.json"
4. Get specific objects from json file
    rId=$(jq '.compute.resourceId' file.json)
    macAddress=$(jq '.network.interface[0].macAddress' file.json)
    publicIpAddress=$(jq '.network.interface[0].ipv4.ipAddress[0].publicIpAddress' file.json)
    privateIpAddress=$(jq '.network.interface[0].ipv4.ipAddress[0].privateIpAddress' file.json)
5. Change permissions of /var/www/html chmod 777
    sudo chmod 777 -R /var/www/html/
6. echo to index.html
    sudo echo "ResourceID - $rId <br> MacAddress - $macAddress <br> privateIPAddress - $privateIpAddress" > /var/www/html/index.html
