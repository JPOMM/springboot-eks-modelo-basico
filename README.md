
# SUBIR IMAGEN A ECR   AWS

Te logeas a AWS con podman
aws ecr get-login-password --region us-east-2 | podman login --username AWS --password-stdin 079726033227.dkr.ecr.us-east-2.amazonaws.com/jpomm/springbooteks

Creas la imagen con Podman
podman build -t springboot-app .

Etiquetas la imagen con Podman
podman tag springboot-app:latest 079726033227.dkr.ecr.us-east-2.amazonaws.com/jpomm/springbooteks:latest

Subes al ECR
podman push 079726033227.dkr.ecr.us-east-2.amazonaws.com/jpomm/springbooteks:latest

Describes la imagen en AWS
aws ecr describe-images --repository-name jpomm/springbooteks

# SUBIR IMAGEN A ACR AZURE










DESPLEGAR EN KUBERNETES


