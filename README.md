# Argo Workflows

## Estructura de directorios

* helm/argo-workflow
  * Instalacion del producto
* helm/workflows
  * Instalacion de workflows y workflow-templates

## Comandos para crear secrets necesarios:
### Github
Se debe crear un secret con la private-key de la Deploy Key agregada en Github.

```shell

# cat github-creds-xxx.yaml
apiVersion: v1
data:
  ssh-private-key: BASE64-PRIVATEKEY
kind: Secret
metadata:
  name: github-creds-xxxxxxxx
  namespace: argo-workflows
type: Opaque

# Aplicamos manifiesto
kubectl apply -f github-creds-xxx.yaml
```

### Harbor
Se debe crear un secret para el robot-account usado en el proyecto.

Ingresar a Harbor > Projects > **proyecto** > Robot Accounts > +New Robot Account

Crear un archivo config.json con las credenciales:

''' config.json
{"auths":{"https://harbor.domain.ar":{"username":"robot$argo-workflows","password":"eyJh............."}}}
'''

Crear el secret:

kubectl create secret generic regcred-<project> --from-file=.dockerconfigjson=config.json --type=kubernetes.io/dockerconfigjson -n argo-workflows

Comandos para crear robot-accounts en harbor
