
# Proyecto de Monitorización con Docker

Este proyecto implementa una solución de monitorización utilizando Zabbix, Grafana y Prometheus con Docker y Docker Compose.

## Contenido

- Zabbix Server
- Zabbix Web con Nginx y PostgreSQL
- Prometheus
- Grafana

---

## Pasos realizados

### 1. Instalación y configuración inicial

Se instalaron los siguientes paquetes en el sistema operativo:
- `docker`
- `docker-compose` o `docker compose plugin` según la versión de Docker.

```bash
sudo apt update
sudo apt install docker.io
sudo apt install docker-compose-plugin
```

### 2. Se añadió el usuario actual al grupo docker

```bash
sudo usermod -aG docker $USER
newgrp docker
```

### 3. Se crearon y depuraron contenedores

Se eliminaron contenedores y volúmenes previos que causaban conflictos:

```bash
docker rm -f <container_id>
docker volume prune -f
docker network prune -f
```

### 4. Se ejecutó el despliegue con Docker Compose

```bash
docker compose up -d
```

Errores comunes resueltos:
- `permission denied while trying to connect to the Docker daemon socket`
- `Conflict. The container name is already in use`
- `undefined volume grafana-storage: invalid compose project`

### 5. Acceso a interfaces

- Prometheus: [http://localhost:9090](http://localhost:9090)
- Zabbix: [http://localhost:8080](http://localhost:8080)
- Grafana: [http://localhost:3000](http://localhost:3000)

### 6. Credenciales

- Zabbix: `Admin / zabbix`
- Grafana: `admin / admin`

### 7. Observaciones

- En Prometheus, para visualizar datos, es necesario configurar correctamente los targets en el `prometheus.yml`.
- En Zabbix, una vez iniciado, muestra alerta si no detecta al agente.
- En Grafana fue necesario asegurarse que los volúmenes definidos existieran en el `docker-compose.yml`.

---

## Autor

Melanie Aponte - Universidad santo tomas

