#ELK + NGINX


This is designed to be a quick out of the box ELK stack that includes password protected https auth to use.

##How To

1. Generate kibana.htpasswd

    htpasswd -c ./kibana.htpasswd username

2. Generate selfsigned certificate

    openssl genrsa -des3 -passout pass:x -out certificate.pass.key 2048
    openssl rsa -passin pass:x -in certificate.pass.key -out certificate.key
    openssl req -new -key certificate.key -out certificate.csr
    openssl x509 -req -days 365 -in certificate.csr -signkey certificate.key -out certificate.crt

3. Run the application

    docker-compose up