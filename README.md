# SiteWhere Kubernetes Orchestration

SiteWhere / Kubernetes integration including Helm Charts

## Chart Details

This chart will do the following:

* Deploy SiteWhere Infrastructure:
  * Deploy [MongoDB](https://www.mongodb.com/)
  * Deploy [Apache Kafka](https://kafka.apache.org/)
  * Deploy [Apache Zookeeper](https://zookeeper.apache.org/)
  * Deploy [Jaeger](https://www.jaegertracing.io/)
  * Deploy [Eclipse Mosquitto](https://mosquitto.org/)
* Deploy SiteWhere Microservices. The table bellow describes the microservices deployed base on the profile selected.

  | Microservice             | Defaul Profile | Minimal Profile |
  | :----------------------- | :------------- | :-------------- |
  | Asset Management         | ✓              | ✓               |
  | Device Management        | ✓              | ✓               |
  | Event Management         | ✓              | ✓               |
  | Event Sources            | ✓              | ✓               |
  | Inbound Processing       | ✓              | ✓               |
  | Instance Managemwnt      | ✓              | ✓               |
  | Outbound Connectors      | ✓              | ✓               |
  | Tenant Management        | ✓              | ✓               |
  | User Management          | ✓              | ✓               |
  | Web Rest                 | ✓              | ✓               |
  | Batch Operations         | ✓              | ✗               |
  | Command Delivery         | ✓              | ✗               |
  | Device Registration      | ✓              | ✗               |
  | Device State             | ✓              | ✗               |
  | Event Search             | ✓              | ✗               |
  | Label Generation         | ✓              | ✗               |
  | Rule Processing          | ✓              | ✗               |
  | Schedule Management      | ✓              | ✗               |
  | Streaming Media          | ✓              | ✗               |
  
* Expose Web Rest port 8080 on an external LoadBalancer

## Installing the Chart

To install SiteWhere Helm Chart follow [this](./charts/README.md) instructions.

## Configuration

The following tables list the configurable parameters of the SiteWhere chart and their default values.

### Microservice Configration

| Parameter                        | Description                                          | Default                          |
| :--------------------------------| :--------------------------------------------------- | :------------------------------- |
| services.profile                 | SiteWhere profile `default` or `minimal`             | `default`                        |
| services.debug                   | Use debug images                                     | `false`                          |
| services.image.registry          | Image registry for microservices container images    | docker.io                        |
| services.image.repository        | Image repository for microservices container images  | sitewhere                        |
| services.image.tag               | Image tag for microservices container images         | `latest`                            |
| services.image.pullPolicy        | Image pull policy for microservices images           | Never                            |
| services.image.imagePullSecrets  | Image pull secrets for microservices images          | `nil`                            |
| services.initContainers          | If `true`, microservices pod will wait for MongoDB.  | `true`                           |

Each _microservice_ has the following configuration:

| Parameter                                            | Description                                     | Default           |
| :----------------------------------------------------| :---------------------------------------------- | :---------------- |
| services._microservice_.enabled                      | `true` if microservice is enabled               | `true`            |
| services._microservice_.image                        | Microservice container images                   | _microservice_    |
| services._microservice_.replicaCount                 | Microservice Replica Count                      | 1                 |
| services._microservice_.service.type                 | Microservice Service Type                       | ClusterIP         |
| services._microservice_.service.grpc.api.port        | Microservice gRPC API Service Port              | 9000              |
| services._microservice_.service.grpc.management.port | Microservice gRPC Management Service Port       | 9001              |

Debug image ports

| Microservice             | JDWP Port      | JMX Port        |
| :----------------------- | :------------- | :-------------- |
| Instance Managemwnt      | 8001           | 1101            |
| User Management          | 8002           | 1102            |
| Tenant Management        | 8003           | 1103            |
| Device Management        | 8004           | 1104            |
| Event Management         | 8005           | 1105            |
| Asset Management         | 8006           | 1106            |
| Event Sources            | 8007           | 1107            |
| Inbound Processing       | 8008           | 1108            |
| Label Generation         | 8009           | 1109            |
| Web Rest                 | 8010           | 1110            |
| Batch Operations         | 8011           | 1111            |
| Command Delivery         | 8012           | 1112            |
| Device Registration      | 8013           | 1113            |
| Device State             | 8014           | 1114            |
| Event Search             | 8015           | 1115            |
| Outbound Connectors      | 8016           | 1116            |
| Rule Processing          | 8017           | 1117            |
| Schedule Management      | 8018           | 1118            |
| Streaming Media          | 8019           | 1119            |

### Infrastructure Configration

#### General Infrastructure Configuration

| Parameter                        | Description                                          | Default                          |
| :------------------------------- | :--------------------------------------------------- | :------------------------------- |
| infra.profile                    | Available values: `mongodb` `cassandra` `influxdb`   | `mongodb`                        |
| infra.image.registry             | Image registry for infrastructure container images   | docker.io                        |
| infra.image.pullPolicy           | Image pull policy for infrastructure images          | IfNotPresent                     |
| infra.image.imagePullSecrets     | Image pull secrets for infrastructure images         | `nil`                            |

#### Jaeger Configuration

| Parameter                         | Description                                    | Default                          |
| :-------------------------------- | :--------------------------------------------- | :------------------------------- |
| infra.jaeger.image                | Jaeger container image                         | jaegertracing/all-in-one         |
| infra.jaeger.replicaCount         | Jaeger Replica Count                           | 1                                |
| infra.jaeger.service.type         | Jaeger Service Type                            | ClusterIP                        |
| infra.jaeger.service.ports.zipkin | Jaeger Zipkin Service Port                     | 9411                             |
| infra.jaeger.service.ports.ui     | Jaeger UI Service Port                         | 16686                            |

#### Eclipse Mosquitto Configuration

| Parameter                             | Description                                     | Default                          |
| :------------------------------------ | :---------------------------------------------- | :------------------------------- |
| infra.mosquitto.image                 | Eclipse Mosquitto container image               | eclipse-mosquitto:1.4.12         |
| infra.mosquitto.replicaCount          | Eclipse Mosquitto Replica Count                 | 1                                |
| infra.mosquitto.service.type          | Eclipse Mosquitto Service Type                  | LoadBalancer                     |
| infra.mosquitto.service.port          | Eclipse Mosquitto Service Port                  | 1883                             |
