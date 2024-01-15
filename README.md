# DEZoomcamp2024


## Testing setup
### Docker
```bash
docker network create pg-network
> 67d626869679d3965655b9d1c374cebf48bd0c4029a91a76d48be7f41f1435dd

docker volume create --name dtc_postgres_volume_local -d local
> dtc_postgres_volume_local

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