# Gatling MQTT Demo - Scala

Proyecto demo para pruebas de rendimiento del protocolo MQTT utilizando [Gatling](https://gatling.io/) con Scala DSL.

## Descripcion

Este proyecto demuestra como realizar pruebas de carga sobre brokers MQTT usando el framework Gatling. Incluye un escenario completo de publicacion/suscripcion (pub/sub) que se conecta al broker publico de HiveMQ.

## Caracteristicas

- Simulacion MQTT con Scala DSL
- Conexion segura TLS al broker
- Escenario pub/sub con validacion de mensajes
- Correlacion de mensajes usando JMESPath
- Reportes HTML detallados

## Requisitos Previos

- **Java JDK 11** o superior
- **Maven 3.8+** (o usar el wrapper incluido `./mvnw`)

Verificar instalacion:

```bash
java -version
./mvnw -version
```

## Estructura del Proyecto

```
.
├── pom.xml                                    # Configuracion Maven
├── mvnw                                       # Maven Wrapper (Unix)
├── mvnw.cmd                                   # Maven Wrapper (Windows)
└── src/
    └── test/
        ├── scala/
        │   └── io/gatling/mqtt/demo/
        │       └── MqttDemoSimulation.scala   # Simulacion principal
        └── resources/
            ├── gatling.conf                   # Configuracion Gatling
            └── logback-test.xml               # Configuracion de logs
```

## Instalacion

1. Clonar el repositorio:

```bash
git clone https://github.com/rcampos09/-gatling-mqtt-demo-scala-rc.git
cd -gatling-mqtt-demo-scala-rc
```

2. Compilar el proyecto:

```bash
./mvnw clean test-compile
```

## Uso

### Ejecutar la simulacion

```bash
./mvnw gatling:test -Dgatling.simulationClass=io.gatling.mqtt.demo.MqttDemoSimulation
```

### Ejecutar con Maven (sin wrapper)

```bash
mvn gatling:test -Dgatling.simulationClass=io.gatling.mqtt.demo.MqttDemoSimulation
```

## Configuracion del Escenario

La simulacion actual (`MqttDemoSimulation.scala`) realiza:

| Paso | Accion | Descripcion |
|------|--------|-------------|
| 1 | Connect | Conexion TLS al broker HiveMQ (puerto 8883) |
| 2 | Subscribe | Suscripcion a un topic unico generado con UUID |
| 3 | Publish | Publicacion de mensaje JSON con ID unico |
| 4 | Check | Validacion de la respuesta recibida |

### Broker por defecto

```scala
mqtt.broker("broker.hivemq.com", 8883)
    .useTls(true)
```

Para usar tu propio broker, modifica estos parametros en `MqttDemoSimulation.scala`.

## Personalizar la Carga

Modificar el metodo `setUp` para ajustar la carga de usuarios:

```scala
// Usuarios simultaneos
setUp(scn.inject(atOnceUsers(10)))

// Rampa de usuarios
setUp(scn.inject(rampUsers(100).during(60.seconds)))

// Carga constante
setUp(scn.inject(constantUsersPerSec(10).during(5.minutes)))
```

## Reportes

Al finalizar la ejecucion, Gatling genera reportes HTML en:

```
target/gatling/<nombre-simulacion>-<timestamp>/index.html
```

Abrir el archivo `index.html` en un navegador para ver:

- Tiempos de respuesta (min, max, promedio, percentiles)
- Solicitudes por segundo
- Usuarios activos en el tiempo
- Distribucion de respuestas

## Tecnologias

| Tecnologia | Version |
|------------|---------|
| Scala | 2.13.17 |
| Gatling | 3.14.9 |
| Gatling MQTT | 3.14.9 |
| Maven | 3.8+ |
| Java | 11+ |

## Autor

**Rodrigo Campos Tapia**

- GitHub: [@rcampos09](https://github.com/rcampos09)

## Licencia

Este proyecto esta bajo la Licencia Apache 2.0 - ver el archivo [LICENSE](LICENSE) para mas detalles.

## Referencias

- [Gatling Documentation](https://docs.gatling.io/)
- [Gatling MQTT Protocol](https://docs.gatling.io/reference/extensions/mqtt/)
- [HiveMQ Public Broker](https://www.hivemq.com/mqtt/public-mqtt-broker/)
