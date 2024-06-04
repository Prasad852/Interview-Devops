## Difference between network load balancer and application load balancer.
Step1 : 
* Create ec2 instance using launch template
* Security group port 22 and port 80 inbound rules 
* with the below userdata
```
#!/bin/bash
sudo yum -y update
sudo yum -y install ruby
sudo yum -y install wget
sudo yum -y install httpd
TOKEN=`curl --silent -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 3600"`
REGION=`curl --silent -H "X-aws-ec2-metadata-token: $TOKEN"  http://169.254.169.254/latest/meta-data/placement/availability-zone | sed 's/\(.*\)[a-z]/\1/'`
INSTANCE_ID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id)
# Log the metadata fetching results for debugging
echo "Instance ID: $INSTANCE_ID" >> /var/log/user-data.log
echo "Region: $REGION" >> /var/log/user-data.log
echo "<h1>$INSTANCE_ID</h1>" > /var/www/html/index.html
# Start the Apache service and enable it to start on boot
systemctl start httpd
systemctl enable httpd
```


## create ASG 

### Network loadbalancer
 * Operates in network layer. uses tcp,tls and udp protocol
*  used for appliction that need millions of request per second
* uses static IP/elastic ip.
  


   