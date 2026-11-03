# RabbitMQ — rama histórica patch-1

Esta rama conserva un experimento académico previo. La documentación de referencia y el entorno principal están en [main](https://github.com/vigneauuu/RabbitMQ/tree/main).

Se retiraron las credenciales fijas de esta rama. Para ejecutar su Compose, copia `.env.example` a `.env` en la carpeta que contiene `docker-compose.yml` y define `RABBITMQ_USER` y `RABBITMQ_PASSWORD` solo en tu equipo. Los clientes reciben esos valores por variables de entorno y los puertos del broker se publican únicamente en localhost.

Las credenciales antiguas permanecen en el historial; si se reutilizaron, deben cambiarse en el broker o servicio correspondiente. No subas `.env` ni capturas de tráfico.
