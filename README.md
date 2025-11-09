```
docker pull mutkoff/ocserv-exporter:latest
```

This is a Docker image with exporter for Prometheus monitoring.

At the moment, this exporter contains 2 metrics:

**user_pass_fail** - is a metric that includes the user's name if the password is entered incorrectly 3 times in 1 minute. 

For example:
user_pass_fail{username="test-user"} 6.0

**ip_user_pass_fail** - is a metric that contains the IP addresses from which unsuccessful attempts were made to enter the account password.

For example:
ip_user_pass_fail{ip="132.144.32.50",username="test-user"} 1.0
ip_user_pass_fail{ip="212.69.120.22",username="test-user"} 5.0

An example of a docker-compose file, where ocserv.log is a file with openconnect server logs (I use the output of the **journalctl -u ocserv** command as logs.). **/root/ocserv.log leave unchanged**
```
services:
  freeipa-exporter:
    image: mutkoff/ocserv-exporter:latest
    ports:
      - '8001:8001'
    volumes:
      - ./ocserv.log:/root/ocserv.log
```
