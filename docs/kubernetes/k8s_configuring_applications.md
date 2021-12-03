# Configuring Applications on Kubernetes

Cloud native applications are supposed to be configured via environment during deployment time.

Kubernetes offers three options to pass configuration to your application:

* [environment variables](#environment-variables)
* [config maps](#configmap) which are key-value pairs managed as separate Kubernetes objects which can be 
consumed as environment variables, as command line arguments or as configuration files by your application
* [secrets](#configmap) which are arbitrary key value pairs with confidential information like credentials 
managed as separate Kubernetes objects which can be used as a source for environment variables.

## Environment Variables

The most common way to pass configuration data into your application is to pass this data as environment variables.
Which environment variables to pass to a container is defined in the container specification of a pod.

Kubernetes supports various options to pass values in environment variables:

* by specifying a value directly
* by referencing a key in a [config map](#configmap)
* by referencing a resource field of the container
* by referencing a key in a [secret](#secret)

### Passing envvars directly

```yaml
    env:
    - name: SPRING_PROFILES_ACTIVE
      value: cloud
```

### Passing envvars from config maps

```yaml
    env:
    # Define the environment variable
    - name: SPECIAL_LEVEL_KEY
      valueFrom:
        configMapKeyRef:
          # The ConfigMap containing the value you want to assign to SPECIAL_LEVEL_KEY
          name: special-config
          # Specify the key associated with the value
          key: special.how
```

### Passing envvars from secrets

```yaml
    env:
    - name: POSTGRES_DB_PASSWORD
      valueFrom:
        secretKeyRef:
          name: postgresql-secret
          key: postgresql-password
```

## ConfigMap

A `config map` represents arbitrary configuration data based on key-value pairs which can be managed as a dedicated 
Kubernetes resource separate from your application. Thus, config maps allow you to decouple configuration artifacts 
from image content to keep containerized applications portable.

Configuration data managed as config maps may be consumed by an application:

* by referencing specific keys as sources for environment variables
* by reading them as files from mounted volumes

!!! danger "Don't use ConfigMaps for confidential information" 
    Since config maps are not encrypted, they are not the right way to pass sensitive confidential data like 
    credentials to an application. Use secrets instead!
    
@see [ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/)

@see [Configure a Pod to Use a ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)

@see [ConfigMap Manifest Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.20/#configmap-v1-core)

## Secret

A `secret` are arbitrary key value pairs with confidential information like credentials managed as separate Kubernetes 
objects which can be used as a source for environment variables.

Kubernetes supports encryption of data contained in secrets at two levels:

* at secret level: secrets are stored in an encrypted format in Kubernetes
* at value level: all values are encrypted (unfortunately using a weak encryption algorithm!)

!!! danger "Protect your secrets from unauthorized access"
    Make sure only authorized people can read secrets from a Kubernetes cluster. Once you've gained access to a
    secret via `kubectl describe secret`, cracking the encryption of the contained values is fairly easy! 

@see [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)

@see [Distribute Credentials Securely Using Secrets](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/)

@see [Secret Manifest Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.20/#secret-v1-core)
