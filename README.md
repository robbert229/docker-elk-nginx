#ELK + NGINX


This is designed to be a quick out of the box ELK stack that includes password protected https auth to use.

##How To

1. Generate kibana.htpasswd

	```
    htpasswd -c ./kibana.htpasswd username
    ```

2. Generate selfsigned certificate

	```
    openssl genrsa -des3 -passout pass:x -out certificate.pass.key 2048
    openssl rsa -passin pass:x -in certificate.pass.key -out certificate.key
    openssl req -new -key certificate.key -out certificate.csr
    openssl x509 -req -days 365 -in certificate.csr -signkey certificate.key -out certificate.crt
    ```

3. Run the application

	```
    docker-compose up
	```

4. Add some data

    One of the easiest ways to start shipping data to elasticsearch is by using beats.
    [https://github.com/elastic/beats](https://github.com/elastic/beats). 
    
    NOTE - When configuring a beat, make sure to 
    * set the port to 5603
    * the username and password to what you specified
    * the protocol to https, 
    * make the tls insecure. (this should not be done in production)

5. Visit `https://127.0.0.1:5602` with your browser, click past the warning ( only if self signing ), and login.

## Details

The setup is pretty simple with docker-compose. Elasticsearch has a volume mounted in the current directory
named esdata that is used to store all of the data that elasticsearch imbibes. Kibana just connects to it, 
and NGINX hosts a reverse proxy on port 5602 that proxies requests to kibana. Elastic search is also on a reverse proxy behind port 5603. Both proxies are password protected and run on https.
