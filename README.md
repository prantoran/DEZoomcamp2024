# DEZoomcamp2024


## Testing setup
### Docker
```bash
docker network create pg-network
```
> 67d626869679d3965655b9d1c374cebf48bd0c4029a91a76d48be7f41f1435dd

```bash
docker volume create --name dtc_postgres_volume_local -d local
```
> dtc_postgres_volume_local

```bash
docker run -it \
    -e POSTGRES_USER="root" \
    -e POSTGRES_PASSWORD="root" \
    -e POSTGRES_DB="my_taxi" \
    -v dtc_postgres_volume_local:/var/lib/postgresql/data \
    -p 5432:5432 \
    --network=pg-network \
    --name pg-database \
    postgres:13
```
In another terminal:
```bash
docker run -it \
    -e PGADMIN_DEFAULT_EMAIL="admin@admin.com" \
    -e PGADMIN_DEFAULT_PASSWORD="root" \
    -p 8080:80 \
    --network=pg-network \
    --name pgadmin \
    dpage/pgadmin4 
```




### GCP
#### Create key
```bash
ssh-keygen -t rsa -f ~/.ssh/gpc -C prantoran -b 2048 
```
#### Connect to virtual instance
```bash
ssh -i ~/.ssh/gpc prantoran@external.ip
```
#### Installs
- Anaconda
```bash
wget https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-x86_64.sh
bash Anaconda3-2023.09-0-Linux-x86_64.sh
```
- Docker
```
sudo apt-get update
sudo apt-get install docker.io
```

```bash
sudo groupadd docker
sudo gpasswd -a $USER docker
sudo service docker restart
docker run hello-world
```
```bash
docker run -it ubuntu bash
```
- docker-compose
```bash
mkdir ${HOME}/bin
```
```bash
export PATH="${HOME}/bin:${PATH}"
```
```bash
cd ${HOME}/bin
wget https://github.com/docker/compose/releases/download/v2.24.0/docker-compose-linux-x86_64 -O docker-compose
chmod +x docker-compose
./docker-compose -v
```
```bash
docker-compose up
```
```bash
docker ps
```
docker-compose.yaml:
```bash
services:
  pgdatabase:
    image: postgres:13
    environment:
      - POSTGRES_USER=root
      - POSTGRES_PASSWORD=root
      - POSTGRES_DB=ny_taxi
    volumes:
      - "./ny_taxi_postgres_data:/var/lib/postgresql/data:rw"
    ports:
      - "5432:5432"
  pgadmin:
    image: dpage/pgadmin4
    environment:
      - PGADMIN_DEFAULT_EMAIL=admin@admin.com
      - PGADMIN_DEFAULT_PASSWORD=root
    ports:
      - "8080:80"
```


- pgcli
```bash
# pip install pgcli
conda install -c conda-forge pgcli
pip install -U mycli
```
```bash
pgcli -h localhost -U root -d ny_taxi
```
List tables
```bash
\dt
```
#### Clone repo
```bash
git clone https://github.com/DataTalksClub/data-engineering-zoomcamp.git
```