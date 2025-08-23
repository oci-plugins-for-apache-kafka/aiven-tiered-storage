# Container images with the Aiven tiered storage plugins for Apache Kafka®

Note: This repository is not part of the Apache Kafka project or related to Aiven!

This repository contains set of OCI artifacts / container images with the Apache Kafka® [tiered storage plugins from Aiven](https://github.com/Aiven-Open/tiered-storage-for-apache-kafka).
These container images should allow you to use it with Strimzi 0.47 and newer using the Kubernetes Image Volumes.
The tag of the container specifies the type of the plugin and its version.

| Plugin               | Container image                                                   |
|----------------------|-------------------------------------------------------------------|
| Filesystem           | `ghcr.io/oci-plugins-for-apache-kafka/aiven-tiered-storage:1.0.0-filesystem` |
| Amazon S3            | `ghcr.io/oci-plugins-for-apache-kafka/aiven-tiered-storage:1.0.0-s3`         |
| Google Cloud Storage | `ghcr.io/oci-plugins-for-apache-kafka/aiven-tiered-storage:1.0.0-gcs`        |
| Azure Blob Storage   | `ghcr.io/oci-plugins-for-apache-kafka/aiven-tiered-storage:1.0.0-azure`      |

## Example `Kafka` custom resource

The following examples show how to mount the plugin into the Strimzi deployment.
Either through the `Kafka` CR:

```yaml
kind: Kafka
metadata:
  name: my-cluster
spec:
  kafka:
    # ...
    template:
      pod:
        volumes:
          - name: aiven-tiered-storage
            image:
              reference: ghcr.io/oci-plugins-for-apache-kafka/aiven-tiered-storage:1.0.0-filesystem
      kafkaContainer:
        volumeMounts:
          - name: aiven-tiered-storage
            mountPath: /mnt/aiven-tiered-storage
        env:
          - name: CLASSPATH
            value: "/mnt/aiven-tiered-storage/*"
```

Or through the `KafkaNodePool` CR:

```yaml
kind: KafkaNodePool
metadata:
  name: broker
spec:
  # ...
  template:
    pod:
      volumes:
        - name: aiven-tiered-storage
          image:
            reference: ghcr.io/oci-plugins-for-apache-kafka/aiven-tiered-storage:1.0.0-filesystem
    kafkaContainer:
      volumeMounts:
        - name: aiven-tiered-storage
          mountPath: /mnt/aiven-tiered-storage
      env:
        - name: CLASSPATH
          value: "/mnt/aiven-tiered-storage/*"
```
