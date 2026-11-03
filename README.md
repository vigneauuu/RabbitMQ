# Laboratorio de mensajería AMQP

Entorno académico con RabbitMQ, Docker Compose y Python para enviar, recibir y observar mensajes AMQP. Proyecto de aprendizaje desarrollado con apoyo de IA.

## Componentes

| Servicio | Función |
| --- | --- |
| `rabbit` | RabbitMQ 4 con panel de administración. |
| `sender` | Emisor en Python; envío periódico o interactivo. |
| `receiver` | Receptor que consume e imprime los mensajes de la cola. |
| `sniffer` | Captura opcional de tráfico TCP 5672 con Scapy. |

El código está en `sender/sender.py`, `receiver/receiver.py` y `sniffer/sniffer.py`. Compose conecta emisor, receptor y broker a `rabbitmq-net` (`192.168.20.0/24`). La cola predeterminada es `Cola_Mensajes`.

## Configuración local

Requiere Docker con el complemento Docker Compose v2. La captura con `network_mode: host` está pensada para Linux y necesita permisos de captura.

1. Copia `.env.example` a `.env`.
2. Completa `RABBITMQ_USER` y `RABBITMQ_PASSWORD` con credenciales exclusivas de este laboratorio.
3. Mantén `.env` fuera de Git. Compose rechaza valores vacíos y los clientes no incluyen contraseñas de respaldo.

Los puertos del broker se publican únicamente en `127.0.0.1`: AMQP en 5672 y administración en 15672.

## Enviar y recibir

Desde la raíz del repositorio:

```bash
docker compose build
docker compose up -d rabbit receiver
docker compose logs -f receiver
```

En otra terminal, elige un modo de envío:

```bash
# Leer mensajes desde el teclado; Ctrl+D termina la entrada.
docker compose run --rm sender -i

# Enviar un mensaje cada 0,3 segundos; Ctrl+C detiene el proceso.
docker compose run --rm sender --frecuencia 0.3 --mensaje "Hola"
```

El emisor admite `--frecuencia/-f`, `--mensaje/-m`, `--interactivo/-i` y `--cola/-q`. Si cambias la cola del emisor, ajusta también `QUEUE` en el receptor.

El [panel de RabbitMQ](http://localhost:15672) utiliza las credenciales de tu `.env`.

## Captura opcional

El sniffer usa la red del host. Identifica la interfaz del laboratorio y ejecuta:

```bash
mkdir -p captura
docker compose run --rm -v "$PWD/captura:/captura" sniffer
```

Selecciona la interfaz cuando el programa la solicite. La captura dura 60 segundos por defecto y se guarda en `captura/captura_amqp.pcap`. Puede incluir autenticación y mensajes en texto claro; no debe subirse al repositorio. Úsalo únicamente en tu entorno de laboratorio.

## Detener el entorno

```bash
docker compose down
```

Esto detiene y retira los contenedores y la red de este proyecto. No se necesita una limpieza global de Docker.

## Alcance

El repositorio incluye envío, recepción y un sniffer básico. No implementa TLS, una evaluación de rendimiento ni una herramienta completa de fuzzing. Las ramas antiguas conservan experimentos previos; `main` es la referencia para estas instrucciones.

Las credenciales antiguas estuvieron publicadas en Git. Retirarlas del código no las revoca: si se reutilizaron en algún broker o cuenta, deben cambiarse allí y deben cerrarse las conexiones anteriores.
