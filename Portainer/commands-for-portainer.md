# portainer
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9005:9000 --name=portainer --restart=always -v \
/var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce

# Uninstall Portainer
docker rm --force portainer
docker rmi portainer/portainer-ce
docker volume rm portainer_data

# Portainer reset password
docker stop "id-portainer-container"
docker run --rm -v portainer_data:/data portainer/helper-reset-password
docker start "id-portainer-container"