
# SUBIR IMAGEN A ECR   AWS

Te logeas a AWS con podman
$ aws ecr get-login-password --region us-east-2 | podman login --username AWS --password-stdin 079726033227.dkr.ecr.us-east-2.amazonaws.com/jpomm/springbooteks

Creas la imagen con Podman
$ podman build -t springboot-app .

Etiquetas la imagen con Podman
$ podman tag springboot-app:latest 079726033227.dkr.ecr.us-east-2.amazonaws.com/jpomm/springbooteks:latest

Subes al ECR
$ podman push 079726033227.dkr.ecr.us-east-2.amazonaws.com/jpomm/springbooteks:latest

Describes la imagen en AWS
$ aws ecr describe-images --repository-name jpomm/springbooteks

# SUBIR IMAGEN A ACR AZURE

Te logeas en azure
$ az login

Te logeas a las subcripcion correcta
$ az account set --subscription bd4eff48-e10b-4bbf-a864-b5ba10bb8f2a

Creas un registo en Azure
$ az acr create --name jpommspringbootaks --resource-group jpomm-registry-rg --sku Basic


Verificar si el registro fue creado 
$ az acr list --output table
     login server:  jpommspringbootaks.azurecr.io

Autentica Podman con Azure ACR:  
primero genera el token 
$ az acr login --name jpommspringbootaks --expose-token
copias el access Token 

$ podman login jpommregistry.azurecr.io -u 00000000-0000-0000-0000-000000000000 -p "<accessToken>"

Construir la imagen 
$ podman build -t springboot-app .

Etqieuta la imagen con el registo de ACR
$ podman tag springboot-app:latest jpommspringbootaks.azurecr.io/springboot-app:latest

Sube la imagen
$ podman push jpommspringbootaks.azurecr.io/springboot-app:latest

Verifica que la imagen esta en el ACR
$ az acr repository list --name jpommspringbootaks --output table

# CONECTARNOS A CLUSTER DE KUBERNETES EN AZURE
Nos conectamos al cluster
$ az aks get-credentials --resource-group jpommResourceGroup --name jpommAKSCluster

Aplicamos el deployment
$ kubectl apply -f k8s.yaml
    deployment.apps/myapp created                                                                                                                                                        
    service/myapp-service created   

Atachar el ACR con el cluster de kubernetes
$ az aks update -n <nombre-del-cluster> -g <nombre-del-grupo> --attach-acr jpommspringbootaks
$ az aks update -n jpommAKSCluster -g jpommResourceGroup --attach-acr jpommspringbootaks
$ kubectl delete pod --all
$ kubectl rollout restart deployment myapp

Varificamos la conexion exterma:
$ kubectl get svc

Dar permisos de salida de puerto 8080 al security group
$ az network nsg rule create --resource-group jpommResourceGroup --nsg-name jpommNSG --name AllowHTTP --priority 100 --direction Inbound --access Allow --protocol Tcp --destination-port-ranges 80









DESPLEGAR EN KUBERNETES


