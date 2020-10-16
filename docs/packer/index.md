# Packer

## Essentials

Packer is an OpenSource-Tool by HashiCorp.
It is mainly used to build identical VM images for multiple platforms based on a single source.
Although Packer differs from configuration management tools like Chef, Puppet and Ansible, it willing to collaborate:

* For example, Packer starts raw base image on given VM platform
* Then, Packer uses Chef, Puppet or Ansible to modify the running VM 
* Finally, Packer exports the built target image using the given image format

Packer supports multiple image formats on various VM platforms like

* AMIs for EC2 (AWS)
* VHD for Azure VMs (Azure)
* OVF for Virtual Box

Originally, Packer DSL was written in JSON but nowadays the HashiCorp Configuration Language (HCL) is supported as well.

No Fuzz again: Packer comes as simple command line tool.

Common use cases for Packer are:

* Adding new base images for virtual machines to public cloud providers
* Extending existing images for virtual machines at public cloud providers

## Concepts and Terminology

### Artifact

An `artifact` represents the result of a single build.

### Build

A `build` is a single Task which produces an image for a specific platform.

### Builder

A `builder` is a Packer component which is able to create an image for a specific platform based on configuration file.

### Command

A `command` specifies a particular job for Packer to execute.

### Post Processor

A `post processor` is a Packer component which accepts a result of a build and transforms it into another type of artifact.

### Provisioner

A `provisioner` is a Packer component which installs and configures software on a running machine before the machine is turned into a static image.

### Template 

A `template` is JSON or HCL configuration file defining one or more builds.

## Commands (excerpt)

| Command | Description |
| --- | --- |
| build | runs a build creating an artifact from a template |
| validate | validates a template |
| inspect | displays the components of a template |

## Resources

[Offical homepage of Packer](https://www.packer.io/)
