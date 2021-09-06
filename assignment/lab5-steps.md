**Steps setup a TLS ingress gateway for the frontend service**

1. **Export ENV**

    ```export DOMAIN_NAME=example.com```

2. **create root certificates**

    ```openssl req -out onlineboutique.$DOMAIN_NAME.csr -newkey rsa:2048 -nodes -keyout onlineboutique.$DOMAIN_NAME.key -subj "/CN=onlineboutique.$DOMAIN_NAME/O=onlineboutique  $DOMAIN_NAME"```

3. **create self signed certificate**

   ```openssl x509 -req -days 365 -CA $DOMAIN_NAME.crt -CAkey $DOMAIN_NAME.key -set_serial 0 -in onlineboutique.$DOMAIN_NAME.csr -out onlineboutique.$DOMAIN_NAME.crt```

4. **Creating the Kubernetes secret**

    ```kubectl create -n istio-system secret tls credentials --key=onlineboutique.example.com.key --cert=onlineboutique.example.com.crt```

5. **Test the above configuration**
 
    ```curl -H "Host:onlineboutique.example.com" --resolve "onlineboutique.example.com:443:$INGRESS_IP" --cacert example.com "https://onlineboutique.example.com:443"```
