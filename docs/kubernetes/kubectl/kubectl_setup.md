# Setting up kubectl to manage Kubernetes clusters

## kubectl config files

All information that kubectl needs to access a particular cluster is stored in kubeconfig YAML files which look like this:

```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN5RENDQWJDZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRFNU1USXhNekV3TURVMU1Gb1hEVEk1TVRJeE1ERXdNRFUxTUZvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBSm5iCnhFQ1ljc0xDMlpWS2g4WXdlbk1LWEtjN2t6bUYydHVZK1NjRFZ1dk1LdWNzQk9oZGV3Kzh0T0hvMWhBbGtkY0QKOEZsQlhsMEc2NkZoSW5zSnd0T3p2U2ZJb2pOa1Q2Z0labnZuMkpYV3B4TWE2bVdaanloMGVhbDNOU3hzcEorNApaVkJ4bUN4UnlpQ0xTN2RGb3BaekFkR3JkeFNpZ0ZwUUp1Nzl5Y1A2VUZ5RGl2Q0ZteVVLcDRUd2xzSks3dkFKCjBLcGs5WWZSYlhhS3I3OG9uQjlkZVdOdkNib0M1aEFKaHNMNHFsOU13eGx3cFRxbkc0WDFOQndrQ05jcXBPMWYKai9xL21ITXUwQVBLaEgzaFFDRElsSTBldEU4ZnkrVjFoL0VLRUpaMFhLYUdWSDg2NWkvZ3lCTHpHVGhNNDR4MwpyaDI0WitBQ3V1bXc0eEtEb2w4Q0F3RUFBYU1qTUNFd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFFdWd6c1pqZFhGVEE5S1BWWXRSclFwdURBWlkKMlU3dVFYdjc0VGxTTU5nVEp3TFhVOC9VSGJCb2xUZ3JxUDF4UlU5M3lXRG9xcEQyZ2hBRzdUUWNwaVd3enBEYgo0QUd3WHNMY3RqemQyMDJYY0JzQ1dPU1V0cTkwR2tNMVM4VlN4S3RVMUd1dDRxNVAwWmZWNThCQXhacjVSeGJnCjZCa1N5SEVONVF1cGRMMlRWb1pYZis3WXNwRFJHcFNVc2M5S1BCYlpwUGZDOXNUU1ZWUkVocitYdXN6aDBSSnAKUkZPakJVMThCZHFXanJIZWVOS2xkQ3RNMWdvR1ozVFB6YWI0YjFGUzIwOS81RzB2NlZxdFNCRjg2MHRVRDg4SgozbXlIenJYbUVrL0xrNEZlNkRJQWg0MFpPT2xBS2ZJYzFyY1J1aDUvNGM3TmNGWXBVWG5yYWp1MjB5WT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
    server: https://10.118.19.238:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: kubernetes-admin
  user:
    client-certificate-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM4akNDQWRxZ0F3SUJBZ0lJYlA1ZjZkdDJGeTB3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB4T1RFeU1UTXhNREExTlRCYUZ3MHlNREV5TVRJeE1EQTJOVEZhTURReApGekFWQmdOVkJBb1REbk41YzNSbGJUcHRZWE4wWlhKek1Sa3dGd1lEVlFRREV4QnJkV0psY201bGRHVnpMV0ZrCmJXbHVNSUlCSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ0tDQVFFQTArZS8zam5KeGliditsUzIKczV4OTBNc0RYRFVleXpxR3pPKytzK0tmMmFuU3c3MDE2YWxSTnJMR0p4VTk2WG0ySkJMMnd5azhPdkZGRml3NApMWFl1RnVXTXhpRjg4dWpCRDlRV2dOcXJjY3FkNVdZRmVYZmsyRE9rSDBRMjJwTUQzRVl6N3VWTDZIRG9wUWZuCitwdnBWcHhxZ3QzMmtOVjZmemxsYldPTXZGSXN1R01jK2JETUkxemFpUGIrNVptUFh1KzlGV255VkdJRDV4cFIKdEYyL2h3c0paN09XeGhsbU16UHhRM0IvdmFXa01UUjBYRFNmK2NIYVFUdmJXYkFldDc4anplYWpQcUZUeFIyTwpKWlJxM0NZaVpUNUxwSW9pYkhWczcycXlmM1ZxeGVVeEJib3VTdEJHMXJyellHTWNOZGFJcm9DbkFNUFZ3THNnCm42bWRtUUlEQVFBQm95Y3dKVEFPQmdOVkhROEJBZjhFQkFNQ0JhQXdFd1lEVlIwbEJBd3dDZ1lJS3dZQkJRVUgKQXdJd0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFCY2VCd2hNTEZBamtkRENBZVdhRVNlaDZneDhobTl6U3dLMgo5K3Z6enRVQkRiR0NnbjlmZ3hHYUZZdEdJQ0ZBMUkycGYybTQwL2tnRjdBMVNDQytiTUZqdCttWmhaU0ZFcUQwCmxNdFp5VzNteDkvZ1c3cVJqOWkwOU1saCszQnNsQVEzY05Ta0hGbkMxc1IvdUVublVkVytVaVFFdE80Z2U1V3IKOWlFY0Ewd1VhTEF0ZndQQzc4ZVVLVy9qV2l1Y25QWjk3aEMyMjYzQlJhN3NXTzNrd1Z5by85bTh5eTQxTlo3ZApGT1dCMXFLQVJ6ZnhuZnhVdTBpcmNVSlFpZUJUMlhnMVYxbUJqVXZLbUdkZkI4Ni9KQTArYjVwWlVZSTArMC80CkpjbE1xc1V6ODA5bTBXWlVmNis3V3dURVEyV2hCOTlvMjhFSmtMemZJZjl5MHhQeGJuMD0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
    client-key-data: LS0tLS1CRUdJTiBSbU0EgUFJJVkFURSBLRVktLS0tLQpNSUlFcEFJQkFBS0NBUUVBMCtlLzNqbkp4aWJ2K2xTMnM1eDkwTXNEWERVZXl6cUd6TysrcytLZjJhblN3NzAxCjZhbFJOckxHSnhVOTZYbTJKQkwyd3lrOE92RkZGaXc0TFhZdUZ1V014aUY4OHVqQkQ5UVdnTnFyY2NxZDVXWUYKZVhmazJET2tIMFEyMnBNRDNFWXo3dVZMNkhEb3BRZm4rcHZwVnB4cWd0MzJrTlY2ZnpsbGJXT012RklzdUdNYworYkRNSTF6YWlQYis1Wm1QWHUrOUZXbnlWR0lENXhwUnRGMi9od3NKWjdPV3hobG1NelB4UTNCL3ZhV2tNVFIwClhEU2YrY0hhUVR2YldiQWV0NzhqemVhalBxRlR4UjJPSlpScTNDWWlaVDVMcElvaWJIVnM3MnF5ZjNWcXhlVXgKQmJvdVN0QkcxcnJ6WUdNY05kYUlyb0NuQU1QVndMc2duNm1kbVFJREFRQUJBb0lCQVFERlFUKzd6N05oL3pENApxQThDbHpRUFBrdThjNzVjay9pVG9NQzJsc2tTUnlGcEVDSzFoZVdSczB6OWFLcWZRYXNwWFhYVEtmbGxMTjZRCnkwem9GTkRHZzV4TmV6TGlzNE8wQmt1RVd3bW8vV0dKL3pRdFpFdmtiZjNqRlE2eTNKT0ZZdHhKRDYxZmpHc2oKNUg0dkxSOUNmb3d2a2d0SnUwOHlTdTE5ckdOL2tKVnNsMlBkQkFabUZFekx0NHdUVElUaCtnVzJDR1lESEVpYgoyL01KRVhBMkZrZkloKzkwN0VvTU1mSUpsNUFpSXRGcmQ2WEZ4UU9oN1UrZlFKUVoxU1lQbHFnd1lwWVdrWktICkd3RjFQRDlTVW01Y0JsNlQyQmpsVm0ybjFIVU00TXVDUmVnZVZReEpJU29QVFNJbWlRbHN0U2hCeU5SSFNIL1EKSE9FM2tSN1pBb0dCQU9renhQRC9CTFdWNnc5b1dIY3VVT0tvTkpCQVQ1KzMvdEFVeTN2TzFIeXl4YVM4a0I4NQo4Q2Q4NmpFV2gxaG5KbTY2Qk1tQzhtSU5QdzA5c2R1VW93Z1IyU0h6RXdIcUhMc0tSNXF0UHdyU0JiSVNrdUFHCklHSjlsaFZPTFk4MlJoN1hZcC9Td3hEYTA2NkhaSit1YSs5SDdkMmF6dkdnRHhSRFVoTkZQNXd2QW9HQkFPaWUKL2hCZG8zQVJWd2FXeTFTdmJ5M2g2bTl0ZUdkY0NCd3lHVlF2T0t0MlFVR1hwTWZOY0tEbEw2SEROUmwvN1dGdQo3R01QaDgrUXZmcWtZZm80Y2NJanduQzFZQ1ZUQ3RMU0Yva3hXNzRwRGZhNGF5d2d0S1J3OWZrWUpSc24vaHNaClZPU1dUY1pKcXhiYUVoQk55VWpyZHRPbXVtL0NzZXhEdFUxSlNJaTNBb0dCQU5Zck1VZVRMYVFHMXlZRFVwdkIKOFk3M282NkhJWmt4eGRjY0FmVG1jc2RDOTdqZlpBMEpqTUQzTzYxeFgwT1ZGL3JBNC95ZFFqVkNyUkZnQTZRQgowZWhyVzlxTi9uclhveU16d2FjUVRNR0hPS3ZkMnYzYklvclJnN0IxWitvS2trTm8wNjZzUlhHSlJyY1dxUmJ0CmZUcjMrRUI1R0cxWDdnRlNBbUtvU2s4SkFvR0FCdDVDNUJyUHE0eG5oR05KWjV1eWJhbGc4WjlLMGNwdTF0NUgKenl1QndkWkJBUDNJT0xvQkhFOElBLyt1ZnEwL1JnUXZhSkZaMGpBVTIrU2ttKzIwdGlXMkpQdkY5ZlFvdXFiSApYRzB2cDBLeERkck9GMFJ6OFBNQTREVHRTNHIzdnJjVndaWUtmOU5IQU9xNVk4L1lKSllITVNLWUdKcW9CRERQCmxwT1dWNnNDZ1lCeldJSHdla0I1bC8zZEo4MkR5ZU5YVUNvcmJsR3VZZXR5ZWxMeTRzWmJxMTV2S0RPa3dUVm8Ka0JVUVBhRktra0Z0UUh1clVKd2REOFFza2MrQXNRN01meXlTTzY1ZTJvOUV4RXZFVTQvZUFpd2x4UUVJQjZaNgpPd3hndEs2VlhIbjQ4UEdsdU1zZjVaQzMwdk5kTmd4cVJOeFR0QzdqK256M2d0VzZYQVBxakE9PQotLS0tLUVORCBSU0EgUFJJVkFURSBLRVktLS0tLQo=
``` 

`kubectl` uses the information in these kubeconfig files to choose a specific cluster and communication with the API server of this particular cluster.

Here a short description of all elements of a kubeconfig file:

* `cluster` defines the name of the cluster (here: __kubernetes__), the URL of its API server, and the CA certificate used for encrypted communication with the cluster.
* `context` groups the access parameters under a convenient name (here: __kubernetes-admin@kubernetes__).
* `current-context` defines the currently active context.
* `user` defines the name of the Kubernetes user used to connect to the cluster and the users credentials, 
which may range from username / password to client certificate / client key 
(like we are using in this particular example). 

!!! danger "Treat kubeconfig files as sensitive information"
    kubeconfig files contain sensitive information: anyone with access to this file automatically has access to your Kubernetes cluster!
    So always keep these files at a secure location (which public cloud storage like OneDrive is not!), 
    avoid to share them with your colleagues, 
    do not store them in git repositories.
    Simply handle them like you would your personal Windows password.
     
## Managing kubeconfig files on your local machine

kubeconfig files are usually provided during cluster creation or by a cluster admin, 
if you do not manage your clusters on your own. The default kubeconfig file is named `config`, but it makes more sense
to apply a naming convention like `${clusterName}.yaml`. All kubeconfig files are stored on your local machine 
at `$HOME/.kube` or `%USERPROFILE%\.kube`. 
If you would like to store them somewhere else or name them differently, you can use the environment variable `KUBECONFIG` to specify a different filename and location.

Let say your cluster is called `manpock8s`. 
According to the naming conventions mentioned before, your kubeconfig file 
would be stored at `$HOME/.kube/manpock8s.yaml`. 
The `KUBECONFIG` environment variable would be set to: 

```shell
export KUBECONFIG=$HOME/.kube/manpock8s.yaml
``` 
or

```shell
set KUBECONFIG=%USERPROFILE%\.kube\manpock8s.yaml
``` 

## Accessing multiple clusters via kubectl contexts

### Managing multiple kubeconfig files

Since running dockerized applications on Kubernetes is the usual way to run cloud native software these days, you will probably have to deal with more than one Kubernetes cluster.
Each cluster comes with its dedicated kubeconfig YAML file.
All kubeconfig files will be stored at directory `$HOME/.kube` or any other location.

To access more than one cluster, you have to list all kubeconfig files in environment var `KUBECONFIG`:

```shell
export KUBECONFIG=$HOME/.kube/cluster1.yaml:/cxp-ide/data/kubernetes/.kube/cluster2.yaml
``` 
or

```shell
set KUBECONFIG=%USERPROFILE%\.kube\cluster1.yaml;C:\cxp-ide\data\kubernetes\.kube\cluster2.yaml
``` 

As you can see the kubeconfig files may be located at different directories.

When `kubectl` is executed, all kubeconfig files listed in `KUBECONFIG` are merged internally into one kubeconfig file containing the access information from all kubeconfig files.

### Switching contexts with kubectl

If want you access a particular cluster you have to switch to the associated cluster context (here: __kubernetes-admin@kubernetes__) using `kubectl`:

```shell
kubectl config use-context kubernetes-admin@kubernetes
```

!!! tipp "Create script files or command files to switch contexts"
    Sometimes, context names are not very easy to memorize or not very descriptive regarding which cluster your are
    actually accessing. So in order to keep things in check, it's a good convention to create script or command files
    running `kubectl config use-context` commands which are named like the cluster you want to access. 

!!! danger "Always make sure that you are accessing the right cluster"
    Before you run any `kubectl` command on a particular cluster, always make sure you switch to the related context.
    `kubectl` will not be asking you for confirmation, if you start to delete stuff on the production cluster while you
    intended to access the development cluster.
     
### Getting information about the current context with kubectl

To get the name of the currently active context, run `kubectl` with the following command:

```shell
$ kubectl config current-context
kubernetes-admin@kubernetes
```

### Getting information about all contexts with kubectl

To get more information about all contexts, you can run:

```shell
kubectl config view
```

## Special case: Azure AKS

### Prerequisites

* You need a local installation of the Azure CLI
* You need access to the Azure subscription containing the Azure AKS cluster
* You need access to the Azure AKS cluster

### Getting the kubeconfig file for an Azure AKS cluster

Run the following command in a console:

````shell
az aks get-credentials -g rg-westeu-cxppltf-dev-stage -n aks-westeu-cxppltf-dev-cxp2022 -f aks-westeu-cxppltf-dev-cxp2022.yaml
````

File `aks-westeu-cxppltf-dev-cxp2022.yaml` will contain the kubeconfig configuration of the Azure AKS cluster `aks-westeu-cxppltf-dev-cxp2022`.

Add this file to the `KUBECONFIG` environment variable of your local machine as mentioned above.

### Setting up Azure AKS login on your local machine

The managed Kubernetes service on Azure called Azure AKS uses a different authentication process like a regular Kubernetes
cluster: Rather than specifying a client certificate / client secret pair, Azure AKS kubeconfig files refer to an Azure specific authentication tool called `kubelogin`:

```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: [_redacted_]
    server: https://aks-westeu-cxppltf-dev-cxp2022-fa7d78f7.hcp.westeurope.azmk8s.io:443
  name: aks-westeu-cxppltf-dev-cxp2022
contexts:
- context:
    cluster: aks-westeu-cxppltf-dev-cxp2022
    user: clusterUser_rg-westeu-cxppltf-dev-stage_aks-westeu-cxppltf-dev-cxp2022
  name: aks-westeu-cxppltf-dev-cxp2022
current-context: aks-westeu-cxppltf-dev-cxp2022
kind: Config
preferences: {}
users:
- name: clusterUser_rg-westeu-cxppltf-dev-stage_aks-westeu-cxppltf-dev-cxp2022
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      args:
      - get-token
      - --environment
      - AzurePublicCloud
      - --server-id
      - [_redacted_]
      - --client-id
      - [_redacted_]
      - --tenant-id
      - [_redacted_]
      - --login
      - devicecode
      command: kubelogin
      env: null
      provideClusterInfo: false
```

Download the kubelogin package from [https://github.com/Azure/kubelogin/releases](https://github.com/Azure/kubelogin/releases) and unzip `kubelogin[.exe]` to a folder in your PATH.

Now whenever you login to Azure AKS for the first time, a browser window will open and you will need to login to the Azure AKS service first.

## Special case: AWS EKS

The managed Kubernetes service on AWS called AWS EKS uses a different authentication process like a regular Kubernetes
cluster: Rather than specifying a client certificate / client secret pair, AWS EKS kubeconfig files refer to an AWS specific authentication tool called `aws-iam-authenticator`:

```yaml
apiVersion: v1
clusters:
- cluster:
    server: https://FD53DF333D7967B9C93897B761D0318D.gr7.eu-central-1.eks.amazonaws.com
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUN5RENDQWJDZ0F3SUJBZ0lCQURBTkJna3Foa2lHOXcwQkFRc0ZBREFWTVJNd0VRWURWUVFERXdwcmRXSmwKY201bGRHVnpNQjRYRFRFNU1UQXdNakE0TVRReE1Wb1hEVEk1TURreU9UQTRNVFF4TVZvd0ZURVRNQkVHQTFVRQpBeE1LYTNWaVpYSnVaWFJsY3pDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRGdnRVBBRENDQVFvQ2dnRUJBTFZVCnpqdnV6cDRUY2lxSWpUMXVPK2xCUVBkVTFNZFJMVjh1SVNIZi91dVBRNzFJcXg4bXhqQTVHS2tYSnM2MHdsaVEKUHlnVmZwTm04dGFQMHRpbXdsNGZUQWZQTldxMnpXVzA1WGY5V2ZHM3FmeE1DTkM2VmdtVEFRaEVVQVRiVnlDWAp2UEtTZVd2Vkd0ZEd6ZXg2OC9FUW9IWU9MQTQreW13YnNxQjVMVjh4dzdoK3dSa3F6R2NadGhpM1diU0tDSkc5CmEyS0VZOTd3SW9xMDQwbW5BVm44U2xoYi9FcmRWNUwvakxVQ0NSMmVPdFdlcWpYNnJWZnBsRG1VUEdlYkR3UGUKaHdCWW1oSVQ0MWxPeEZyQ0hTbXZoc2JhOE16ZHgySUFqWGVFd3BIek56NjRBdlA0ZjF3L0N2QUF0c09QMmwySQp2MjQ1VDc2ekwzdTlsekZjbmhrQ0F3RUFBYU1qTUNFd0RnWURWUjBQQVFIL0JBUURBZ0trTUE4R0ExVWRFd0VCCi93UUZNQU1CQWY4d0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFHSFR1d2plUFpQZXViNkRHdEwzSlUwZ09BMFYKcVdIY0kySFlkb053ZkV4MXlWNXM3UVdRYmpYUmo2aEdkbVRJNENQUUwyT3EyOERSRWxBK2s4SWdxNDJjbXlFWQpOZGplN2VpREVkY24zaDR2M1BsZGgrSTZmSUI1SlZMWis1Q2tKV0d5VFhxSVhNMFZyMkVaY1AvQ0xuUEEwVEdZCnJXL0k2VTU5MjFFWlZhTVZBMEJYU0Q3Uktka3JVR3FHTkhBcFdHS1hFZUlQWEVNTkV6ck5oOUo0UFJrZmNlRngKRGhuZ0ltOGpTNjBaaG9UdCtpOXg3Y3pHNXJIcUpydzFOUkZOWmwvR3RreUtCZUNoU1R1NFdBVklyQlpCRDhiMwpFQTBvVDBpRDJXQWZRd1VFNlZVeHdLNE9xVnE2b3lSTjMyV2Jkc3FPd3pZVjBqS0hpSTJ4Qy9iOVpnTT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: aws
  name: aws
current-context: aws
kind: Config
preferences: {}
users:
- name: aws
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1alpha1
      command: aws-iam-authenticator
      args:
        - "token"
        - "-i"
        - "eks-eu-central-1-k8seksdemo"
``` 

`aws-iam-authenticator` uses the current AWS API user to retrieve an access token for a specific cluster first. 
Then this access token is used to actually authenticate on a specific EKS cluster.

Therefore, the following prerequisites must be met when dealing with AWS EKS clusters:

* you need an AWS API user with an access key/secret key pair which is authorized to access this particular EKS cluster
* you need to setup this AWS API user on your local machine (see: [Setting up a Programmatic User Account](../../aws/iam/aws_programmatic_user_setup_v2.md))
* you need to switch to the AWS profile referring to this particular AWS API user (if you are using more than one AWS API user)
* you need to install the AWS command line interface (see: [Installing the AWS CLI version 2 on Windows](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-windows.html)) and the `aws-iam-authenticator` on your local machine (see: [Installing aws-iam-authenticator](https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html))