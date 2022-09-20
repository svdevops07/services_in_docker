# Start your Private Docker Registry v2.7.1 with TLS

## 1. Make certificates with certbot for uour domain

```
certbot your-domain.comp.com
```

## 2. Change certificates to new format

```
cd //etc/letsencrypt/live/your-domain.comp.com/
cp privkey.pem domain.key
cat privkey.pem chain.pem > domain.crt
chmod 777 domain.key
chmod 777 domain.crt
```

## 3. Make directory for registry's files and generate user/pass

```
mkdir /home/my-docker-registry
cd /home/my-docker-registry
mkdir auth
docker run   --entrypoint htpasswd   registry:2 -Bbn testuser testpassword > auth/htpasswd
```

## 4. Run Private Registry in Docker
```
docker run -d -p 443:5000 --restart=always --name my-docker-registry \
-v /etc/letsencrypt/live/your-domain.comp.com:/certs \
-v /home/my-docker-registry:/var/lib/registry \
-e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
-e REGISTRY_HTTP_TLS_KEY=/certs/domain.key -e REGISTRY_AUTH=htpasswd \
-e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
-e REGISTRY_AUTH_HTPASSWD_PATH=/var/lib/registry/passfile registry:2.7.1
```
