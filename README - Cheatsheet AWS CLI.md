Cheatsheet AWS CLI
==================

En este documento ha sido creado y producto de un curso con Raul del [CRN](https://cftic.centrosdeformacion.empleo.madrid.org/), junto con un estudio de la documentación de AWS CLI. 

- [Apuntes del curso](https://nvarona.x10.bz/Apuntes_AWS_2025.pdf)
- [Artículo en el Blog de "Apuntes AWS y resumen de sus servicios"](https://nvarona.x10.bz/apuntes-aws-y-resumen-de-sus-servicios)

El curso puede ser base preparatorio para 4 [certificaciones oficiales](https://aws.amazon.com/es/certification/) de AWS:

- Cloud Practitioner Fundational(6 meses)
- Developer Associate (TI local o nube)
- Solutions Architect (2 años)
- DevOps EngineerSysOps (2 años)

# Índice 📎
- [Cheatsheet AWS CLI](#cheatsheet-aws-cli)
- [Índice 📎](#índice-)
  - [¿Qué es la CLI de AWS?](#qué-es-la-cli-de-aws)
  - [Configurar](#configurar)
  - [API Gateway](#api-gateway)
  - [Amplify](#amplify)
  - [CloudFront](#cloudfront)
  - [CloudWatch](#cloudwatch)
  - [CloudTrail](#cloudtrail)
  - [Cognito](#cognito)
  - [DynamoDB](#dynamodb)
  - [EBS](#ebs)
  - [EC2](#ec2)
  - [FSC](#fsc)
  - [ECR](#ecr)
  - [ECS](#ecs)
  - [EKS](#eks)
  - [ElastiCache](#elasticache)
  - [ELB](#elb)
  - [Grupo IAM](#grupo-iam)
  - [Usuario IAM](#usuario-iam)
  - [Lambda](#lambda)
  - [RDS](#rds)
  - [Route53](#route53)
  - [S3](#s3)
  - [SNS](#sns)
  - [SQS](#sqs)
  - [Filtros de las filas](#filtros-de-las-filas)
  - [Filtros de columnas. Opción Query](#filtros-de-columnas-opción-query)


¿Qué es la CLI de AWS?
----------------------

AWS CLI significa Amazon Web Services Command Line Interface. A la hora de gestionar tus servicios de AWS hay unas cuantas opciones en cuanto a herramientas. Dos de las opciones más comunes son el uso de la consola de AWS, o la CLI de AWS. La consola de AWS es una interfaz web a la que se accede para gestionar los servicios de AWS. En contraste con la consola de AWS es AWS CLI. Es una gran herramienta para gestionar los recursos de AWS en diferentes cuentas, regiones y entornos desde la línea de comandos. Te permite controlar los servicios manualmente o crear automatizaciones con scripts.

Si aún no has instalado AWS CLI, comienza en la [Guía de instalación de AWS CLI de Amazon](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html).

**Instalación en Linux**

	curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
	unzip awscliv2.zip
	sudo ./aws/install

**Instalación en MacOS**

    curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
    sudo installer -pkg AWSCLIV2.pkg -target /

Para ver la versión:

    $ which aws
    /usr/local/bin/aws
    $ aws –version

**Consejo 1 - Usa la función de completar comandos.**

Creemos que la mejor hoja de trucos que puedes tener para AWS CLI es la función de completar comandos. Le permite utilizar la tecla Tab para completar un comando parcialmente introducido. Completará tu comando o mostrará una lista de comandos sugeridos. No siempre se instala automáticamente, por lo que tendrás que configurarlo manualmente. Aquí está la [guía de AWS](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-completion.html) para ponerlo en marcha.

**Consejo 2 - Usa el comando help.**

Cuando necesites un poco de ayuda extra sólo tienes que apoyarte en el comando de ayuda de la CLI de AWS para obtener documentación detallada sobre lo que está disponible. Para utilizar este comando sólo tienes que añadir help al final del nombre del comando. Por ejemplo, si haces 'aws help', te mostrará las opciones generales de la CLI de AWS y una lista de todos los servicios. Si necesitas ver todos los comandos disponibles para AWS EC2 específicamente, escribirías `aws ec2 help`. Se convertirá en una gran ayuda para que te conviertas en un profesional de la CLI de AWS.

**Consejo 3 - Utilizar jq.**

Esta hoja de trucos utiliza jq, un procesador JSON de línea de comandos ligero y flexible. Recomendamos encarecidamente su uso para la CLI de AWS. Puedes encontrar más información sobre él en el [repositorio de Github para ello](https://stedolan.github.io/jq/).

Configurar
----------

Create profiles

    aws configure --profile profilename

Formato de salida

    aws configure output format {json, yaml, yaml-stream, text, table}

Especificar Region AWS

    aws configure region (region-name)

Si queremos hacerlo todo a la vez

	aws configure

Pedirá 4 datos: El ID de la clave, la clave, la region por defecto y el formato de salido por defecto (Por defecto es text.)

Mostrar el usuario conectado

	aws sts get-caller-identity

API Gateway
-----------

Listar *API Gateway IDs* y nombres

    aws apigateway get-rest-apis | jq -r ‘.items[ ] | .id+” “+.name’

Listar *API Gateway keys*

    aws apigateway get-api-keys | jq -r ‘.items[ ] | .id+” “+.name’

Listar *API Gateway domain names*

    aws apigateway get-domain-names | jq -r ‘.items[ ] | .domainName+” “+.regionalDomainName’

Listar recursos para *API Gateway*

    aws apigateway get-resources --rest-api-id ee86b4cde | jq -r ‘.items[ ] | .id+” “+.path’

Encontrar el recurso Lambda para *API Gateway*

    aws apigateway get-integration --rest-api-id (id) --resource-id (resource id) --http-method GET | jq -r ‘.uri’

Amplify
-------

Listar las aplicaciones de Amplify y el repositorio de fuentes

    aws amplify list-apps | jq -r '.apps[ ] | .name+" "+.defaultDomain+"

CloudFront
----------

Listar las distribuciones y orígenes de CloudFront

    aws cloudfront list-distributions | jq -r '.DistributionList.Items[ ] | .DomainName+" "+.Origins.Items[0].DomainName'

Crear una nueva invalidación

    aws cloudfront create-invalidation [distribution-id]

CloudWatch
----------

Listar información sobre una alarma

    aws cloudwatch describe-alarms | jq -r '.MetricAlarms[ ] | .AlarmName+" "+.Namespace+" "+.StateValue'

Eliminar una o varias alarmas (puede eliminar hasta 100 a la vez)

    aws cloudwatch delete-alarms --alarm-names (alarmnames)

CloudTrail
----------

Listar la trails

	aws cloudtrail list-trails

Mostrar propiedades

	aws cloudtrails describe-trails –trail-name-list nombre nombre-bucket

Crear un trail en un bucket existente. Por defecto, guarda los datos de administración.

	aws cloudtrail create-trail --name Nombre-Trail –s3-bucket-name nombre-bucket 

Eliminar un trail

	aws cloudtrail delete-trail --name nombre-del-trail


Cognito
-------

Listar los IDs y nombres de los grupos de usuarios

    aws cognito-idp list-user-pools --max-results 60 | jq -r '.UserPools[ ] | .Id+" "+.Name'

Listar el teléfono y el correo electrónico de todos los usuarios

    aws cognito-idp list-users --user-pool-id (resource) | jq -r '.Users[ ].Attributes | from_entries | .sub + " " + .phone_number + " " + .email'

DynamoDB
--------

Listar las tablas de DynamoDB

    aws dynamodb list-tables | jq -r .TableNames [ ]

Obtener todos los elementos de una tabla

    aws dynamodb scan --table-name events

Obtener el recuento de elementos de una tabla

    aws dynamodb scan --table-name events --select count | jq .ScannedCount

Obtener el elemento utilizando la clave

    aws dynamodb get-item --table-name events --key '{"email"""email@example.com"}}'

Obtener campos específicos de un elemento

    aws dynamodb get-item --table-name events --key '{"email"" email@example.com"}}' --attributes-to-get event_type

Eliminar un elemento utilizando la clave

    aws dynamodb delete-item --table-name events --key '{"email""email@domain.com"}}'

EBS
---

Completar una instantánea

    aws ebs complete-snapshot (snapshot-id)

Iniciar una instantánea

    aws ebs start-snapshot --volume-size (valor)

Obtener un bloque de Snapshot

    aws ebs get-snapshot-block

    --snapshot-id (valor)

    --block-index (valor)

    --block-token (valor)

EC2
---

Listar el ID, el tipo y el nombre de la instancia

    aws ec2 describe-instancias | jq -r '.Reservas[].Instances[]|.InstanceId+" "+.InstanceType+" "+(.Etiquetas[] | select(.Clave == "Nombre").Valor)'

Lista de instancias con dirección IP pública y nombre

    aws ec2 describe-instances --query 'Reservas[*].Instances[?not_null(PublicIpAddress)]' | jq -r '.[][]|.PublicIpAddress+" "+(.Tags[]|select(.Key=="Name").Value)'


Crear una instancia - se necesita la ID de un AMI. Luego se debe indicar el tipo de instancia, la Key Pair y el grupo de seguridad. Además indicamos cuantas instancias queremos. Ejemplo:

	aws ec2 run-instances --image-id ami-02d0b1ffa5f16402d --instance-type t3.micro --key-name ServidorPruebasAWS2 --security-groups ServidoresWEB --count 1ç

Stop instancia

	aws ec2 stop-instances --instance-ids i-00a3750814c5340e9

Start instancia

	aws ec2 start-instances --instance-ids i-00a3750814c5340e9

Terminar instancia

	aws ec2 terminate-instances --instance-ids i-00a3750814c5340e9

Mostrar AMIs

	aws ec2 describe-images --owner self

La opción `--owner self` se debe poner para que tan solo salga las AMIs propias y no todas a las que se tiene acceso, que son todas las públicas. Ejemplo con una query compleja

	aws ec2 describe-images --owner self --query 'Images[].[{"ID imagen":ImageId},{"Nombre":Name},{"ID SnapShot":BlockDeviceMappings[].Ebs.SnapshotId}]'

Crear una AMI desde una instancia que este corriendo - Primero hay que examinar la ID de la instancia de la que se quiere crear el AMI.

	aws ec2 describe-instances 

Se podrá filtrar con --query o --filter para localizar la instancia concreta. Luego hacemos el comando:

aws ec2 create-image --instance-id i-0e060585afc6bcab9 --name AMIpersonalizadaConWeb

Para comprobar si es correcto con query

	aws ec2 describe-images --owner self --query "Images[].[Name,ImageId,BlockDeviceMappings[].Ebs.SnapshotId,State]"

Mostrar volumenes

	aws ec2 describe-volumes

Para ver solo los ids de los volumenes

	aws ec2 describe-volumes  --query 'Volumes[].VolumeId'

Para ver a donde están asociadas

	aws ec2 describe-volumes --query 'Volumes[].[{"Id del Volumen":VolumeId},{"ID instancia asociada":Attachments[].InstanceId}]'

Crear volumen

	aws ec2 create-volume --availability-zone eu-west-3b --size 5 --volume-type gp2

Adjuntar volumen a una instancia

	aws ec2 attach-volume --device /dev/sdf --instance-id i-0e060585afc6bcab9 --volume-id vol-0c956f9595c594bb9

Para desmontar un volumen

    aws ec2 detach-volume --volume-id vol-0c956f9595c594bb9

Borrar un volumen

    aws ec2 delete-volume --volume-id vol-0c956f9595c594bb9

Lista de VPCs y bloque de IPs CIDR

    aws ec2 describe-vpcs | jq -r '.Vpcs[]|.VpcId+" "+(.Tags[]|select(.Key=="Name").Value)+" "+.CidrBlock'

Lista de subredes para una VPC

    aws ec2 describe-subnets --filters Name=vpc-id,Values=vpc-0d1c1cf4e980ac593 | jq -r '.Subnets[]|.SubnetId+" "+.CidrBlock+" "+(.Tags[]|select(.Key=="Name").Value)'

Lista de grupos de seguridad

    aws ec2 describe-security-groups | jq -r '.SecurityGroups[]|.GroupId+" "+.GroupName'

Mostrar los grupos de seguridad de una instancia

    aws ec2 describe-instancias --instancia-ids i-0dae5d4daa47fe4a2 | jq -r '.Reservas[].Instancias[].SecurityGroups[]|.GroupId+" "+.GroupName'

Editar los grupos de seguridad de una instancia

    aws ec2 modify-instance-attribute --instance-id i-0dae5d4daa47fe4a2 --groups sg-02a63c67684d8deed sg-0dae5d4daa47fe4a2

Mostrar las reglas del grupo de seguridad como *FromAddress* y *ToPort*

    aws ec2 describe-security-groups --group-ids sg-02a63c67684d8deed | jq -r '.SecurityGroups[].IpPermissions[]|. as $parent|(.IpRanges[].CidrIp+" "+($parent.ToPort|tostring))'

Añadir regla al grupo de seguridad

    aws ec2 authorize-security-group-ingress --group-id sg-02a63c67684d8deed --protocolo tcp --puerto 443 --cidr 35.0.0.1

Eliminar la regla del grupo de seguridad

    aws ec2 revoke-security-group-ingress --group-id sg-02a63c67684d8deed --protocolo tcp --port 443 --cidr 35.0.0.1

Editar las reglas del grupo de seguridad

    aws ec2 update-security-group-rule-descriptions-ingress --group-id sg-02a63c67684d8deed --ip-permissions 'ToPort=443,IpProtocol=tcp,IpRanges=[{CidrIp=202.171.186.133/32,Description=Home}]'

Eliminar grupo de seguridad

    aws ec2 delete-security-group --group-id sg-02a63c67684d8deed

FSC
---

Ejemplo backup:

	aws fsx create-backup –file-system-id xxxxxxxxxxxxx

Mostrar backups

	aws fsx describe-backups

ECR
---
Describir el registro privado:

	aws ecr describe-registries

Describir repositorios

	aws ecr describe-repositories

Listar las imágenes de un repositorio

	aws ecr list-images –repository-name <nombre-repositorio>

Crear un repositorio

	aws ecr create-repository --repository-name <nombre-repositorio>

Se pueden subir imágenes con AWS CLI pero el mismo AWS recomienda que se use el comandos DOCKER. 

Los comandos que tienen que ver con el subcomando ecr-public solo funcionan en la region us-east-1, con lo que hay que especificarla  para poder utilizarlos.

Describir el registro público

	aws ecr-public describe-registries --region us-east-1

Mostrar los repositorios

	aws ecr-public describe-repositories --region us-east-1



ECS
---

Crear un clúster ECS

    aws ecs create-cluster --cluster-name=NAME --generate-cli-skeleton

Crear un servicio ECS

    aws ecs create-service


EKS
---

Crear un cluster

    aws eks create-cluster --name (nombre del cluster)

Eliminar un cluster

    aws eks delete-cluster --nombre (nombre del cluster)

Listar información descriptiva de un cluster

    aws eks describe-cluster --nombre (nombre del cluster)

Listar clusters en su región por defecto

    aws eks list-clusters

Etiquetar un recurso

    aws eks tag-resource --resource-arn (resource_ARN) --tags (etiquetas)

Desetiquetar un recurso

    aws eks untag-resource --resource-arn (resource_ARN) --tag-keys (tag-key)

Listar los grupos de nodos de un clúster

	aws eks list-nodegroups –cluster-name <nombre-cluster>

Información sobre un grupo de nodos

	aws eks describe-nodegroup --cluster-name <nombre-cluster> --nodegroup <nombre-nodegroup>

ElastiCache
-----------

Obtener información sobre un clúster de caché específico

    aws elasticache describe-cache-clusters | jq -r '.CacheClusters[ ] | .CacheNodeType+" "+.CacheClusterId'

Lista de grupos de replicación de ElastiCache

    aws elasticache describe-replication-groups | jq -r '.ReplicationGroups [ ] | .ReplicationGroupId+" "+.NodeGroups[ ].PrimaryEndpoint.Address'

Lista de instantáneas de ElastiCache

    aws elasticache describe-snapshots | jq -r '.Snapshots[ ] | .SnapshotName'

Crear una instantánea de ElastiCache

    aws elasticache create-snapshot --snapshot-name backend-login-hk-snap-1 --replication-group-id backend-login-hk --cache-cluster-id backend-login-hk

Eliminar la instantánea de ElastiCache

    aws elasticache delete-snapshot --snapshot-name login-snap-1

Aumentar/reducir la réplica de ElastiCache

    aws elasticache increase-replica-count --replication-group-id backend-login --apply-immediately

    aws elasticache decrease-replica-count --replication-group-id backend-login --apply-immediately

ELB
---

Lista de nombres de host del ELB

    aws elbv2 describe-load-balancers --query 'LoadBalancers[*].DNSName' | jq -r 'to_entries[ ] | .value'

Listar los ARNs de los ELBs

    aws elbv2 describe-load-balancers | jq -r '.LoadBalancers[ ] | .LoadBalancerArn'

Lista de ARNs de grupos de destino de ELB

    aws elbv2 describe-target-groups | jq -r '.TargetGroups[ ] | .TargetGroupArn'

Buscar instancias para un grupo de destino

    aws elbv2 describe-target-health --target-group-arn arn:aws:elasticloadbalancing:ap-northwest-1:20394823094:targetgroup/wordpress-ph/203942b32a23 | jq -r '.TargetHealthDescriptions[ ] | .Target.Id'

Mostrar Listeners

	aws elbv2 describe-listeners --load-balancer-arn arn:aws:elasticloadbalancing:eu-west-3:992365247711:loadbalancer/app/BalanceadorWEBParis/55c6c9cc0aefe3dc

Mostrar regla

	aws elbv2 describe-rules --listener-arn arn:aws:elasticloadbalancing:eu-west-3:992365247711:listener/app/BalanceadorWEBParis/55c6c9cc0aefe3dc/12aa82e9221fed60

Identificar una regla en concretoç

	aws elbv2 delete-rule --rule-arn arn:aws:elasticloadbalancing:eu-west-3:992365247711:listener-rule/app/BalanceadorWEBParis/55c6c9cc0aefe3dc/12aa82e9221fed60/76519d748f2250a1


Grupo IAM
---------

Lista de grupos

    aws iam list-groups | jq -r .Groups[ ].GroupName

Añadir/borrar grupos

    aws iam create-group --group-name (groupName)

Listar políticas y ARNs

    aws iam list-policies | jq -r '.Policies[ ]|.PolicyName+" "+.Arn'

    aws iam list-policies --scope AWS | jq -r '.Policies[ ]|.PolicyName+" "+.Arn'

    aws iam list-policies --scope Local | jq -r '.Policies[ ]|.PolicyName+" "+.Arn'

Listar usuarios/grupos/roles para una política

    aws iam list-entities-for-policy --policy-arn arn:aws:iam:2308345:policy/example-ReadOnly

Listar políticas para un grupo

    aws iam list-attached-group-policies --group-name (groupname)

Para crear una políticas:

	aws iam create-policy --policy-name politica-desde-cli--01 –policy-document [Podría subirlo desde un bucket con el arn y los permisos adecuado. O desde local con file://nombredocumento]

Añadir una política a un grupo

    aws iam attach-group-policy --nombre-del-grupo (nombre-del-grupo) --policy-arn arn:aws:iam::aws:policy/examReadOnlyAccess

Añadir un usuario a un grupo

    aws iam add-user-to-group --group-name (groupname) --user-name (username)

Eliminar un usuario de un grupo

    aws iam remove-user-from-group --nombre-grupo (nombre-grupo) --nombre-usuario (nombre-usuario)

Listar los usuarios de un grupo

    aws iam get-group --nombre-del-grupo (nombre-del-grupo)

Listar los grupos de un usuario

    aws iam list-groups-for-user --nombre-usuario (nombre-usuario)

Adjuntar/eliminar la política a un grupo

    aws iam attach-group-policy --nombre-del-grupo (nombre-del-grupo) --policy-arn arn:aws:iam::aws:policy/DynamoDBFullAccess

    aws iam detach-group-policy --group-name (groupname) --policy-arn arn:aws:iam::aws:policy/DynamoDBFullAccess

Usuario IAM
--------

Listar *userId* y *UserName*

    aws iam list-users | jq -r '.Users[ ]|.UserId+" "+.UserName'

Obtener un solo usuario

    aws iam get-user --user-name (nombre-de-usuario)

Añadir usuario

    aws iam create-user --user-name (nombre-de-usuario)

Eliminar usuario

    aws iam delete-user --user-name (nombre-usuario)

Listar las claves de acceso del usuario

    aws iam list-access-keys --user-name (nombre-de-usuario) | jq -r .AccessKeyMetadata[ ].AccessKeyId

Eliminar la clave de acceso del usuario

    aws iam delete-access-key --user-name (nombre-de-usuario) --id-de-clave-de-acceso (accessKeyID)

Activar/desactivar la clave de acceso del usuario

    aws iam update-access-key --status Active --user-name (username) --access-key-id (access key)

    aws iam update-access-key --status Inactive --user-name (username) --access-key-id (access key)

Generar una nueva clave de acceso para el usuario

    aws iam create-access-key --user-name (nombre-de-usuario) | jq -r '.AccessKey | .AccessKeyId+" "+.SecretAccessKey'

Lambda
------

Lista de funciones Lambda, tiempo de ejecución y memoria

    aws lambda list-functions | jq -r '.Functions[ ] | .FunctionName+" "+.Runtime+" "+(.MemorySize|tostring)'
    

Lista de capas de Lambda

    aws lambda list-layers | jq -r '.Layers[ ] | .LayerName'

Lista de eventos de origen para Lambda

    aws lambda list-event-source-mappings | jq -r '.EventSourceMappings[ ] | .FunctionArn+" "+.EventSourceArn'

Descargue el código Lambda

    aws lambda get-function --function-name DynamoToSQS | jq -r .Code.Location

RDS
---

Lista de clusters de BD

    aws rds describe-db-clusters | jq -r '.DBClusters[ ] | .DBClusterIdentifier+" "+.Endpoint'

Listar las instancias de la DB

    aws rds describe-db-instances | jq -r '.DBInstances[ ] | .DBInstanceIdentifier+" "+.DBInstanceClass+" "+.Endpoint.Address'

Crear snapshot de la instancia de la BBDD

    aws rds create-db-snapshot --db-snapshot-identifier snapshot-1 --db-instance-identifier dev-1

Mostrar snapshot

    aws rds describe-db-snapshots --db-snapshot-identifier snapshot-1 --db-instance-identifier general

Tomar la instantánea del clúster de la BBDD

    aws rds create-db-cluster-snapshot --db-cluster-snapshot-identifier

Crear una base de datos RDS de tipo Mysql

	aws rds create-db-instance –db-instance-identifier db-20 --db-instance-class db.t3.micro --engine mysql –master-username admin1 --master-user-password lepanto1 --allocated-storage 20

Borrar el snapshot

	aws rds delete-db-snapshot --db-snapshot-identifier snp-11

Borrar la Base de datos

	aws rds delete-db-instance --db-instance-identifier db-02 --skip-final-snapshot

También se puede pedir el nombre de la snapshot, si sabemos concretarla, meterla en una variable bash y utilizarla para borrar:

	snapshot=$(aws rds describe-db-snapshots --snapshot-type manual --query DBSnapshots[*].DBSnapshotIdentifier --output text)

	aws rds delete-db-snapshot --db-snapshot-identifier $snapshot


Route53
-------

Crear zona alojada

    aws route53 create-hosted-zone --name exampledomain.com

Eliminar zona alojada

    aws route53 delete-hosted-zone - -id ejemplo

Obtener zona alojada

    aws route53 get-hosted-zone --id ejemplo

Listar zonas alojadas

    aws route53 list-hosted-zones

Crear un conjunto de registros - Para ello, primero tendrá que crear un archivo JSON con una lista de elementos de cambio en el cuerpo y utilizar la acción CREATE. Por ejemplo, el archivo JSON tendría el siguiente aspecto

    {
         "Comment": "CREATE/DELETE/UPSERT a record",
         "Changes": [{
         "Action": "CREATE",
              "ResourceRecordSet":{
                   "Name": "a.example.com",
                   "Type": "A",
                   "TTL": 300,
              "ResourceRecords":[{"Value":"4.4.4.4"}]
    }}]
    }
    

Una vez que tengas un archivo JSON con la información correcta como la anterior podrás introducir el comando

    aws route53 change-resource-record-sets --hosted-zone-id (zone-id) --change-batch file://exampleabove.json

Actualizar un conjunto de registros - Para ello, primero tendrá que crear un archivo JSON con una lista de elementos de cambio en el cuerpo y utilizar la acción UPSERT. Esto creará un nuevo conjunto de registros con el valor especificado, o actualizará un conjunto de registros si ya existe. Por ejemplo, el archivo JSON tendría el siguiente aspecto

    {
         "Comment": "CREATE/DELETE/UPSERT a record",
         "Changes": [{
         "Action": "UPSERT",
              "ResourceRecordSet":{
                   "Name": "a.example.com",
                   "Type": "A",
                   "TTL": 300,
              "ResourceRecords": [{"Value":"4.4.4.4"}]
    }}]
    }
    

Una vez que tengas un archivo JSON con la información correcta como la anterior podrás introducir el comando

    aws route53 change-resource-record-sets --hosted-zone-id (zone-id) --change-batch file://exampleabove.json

Eliminar un conjunto de registros - Para ello, primero tendrá que crear un archivo JSON con una lista de los valores del conjunto de registros que desea eliminar en el cuerpo y utilizar la acción DELETE. Por ejemplo, el archivo JSON tendría el siguiente aspecto

    {
         "Comment": "CREATE/DELETE/UPSERT a record",
         "Changes": [{
         "Action": "DELETE",
              "ResourceRecordSet": {
                   "Name": "a.example.com",
                   "Type": "A",
                   "TTL": 300,
              "ResourceRecords": [{"Value":"4.4.4.4"}]
    }}]
    }
    

Una vez que tengas un archivo JSON con la información correcta como la anterior podrás introducir el siguiente comando.

    aws route53 change-resource-record-sets --hosted-zone-id (zone-id) --change-batch file://exampleabove.json

S3
--

Lista de buckets

    aws s3 ls

Listar archivos en un Bucket

    aws s3 ls s3://mybucket

Crear un Bucket

    aws s3 mb s3://bucket-name
    make_bucket: bucket-name

Borrar Bucket

    aws s3 rb s3://bucket-name --force

Descargar el objeto S3 en local

    aws s3 cp s3://bucket-name
    download: ./backup.tar from s3://bucket-name/backup.tar

Subir el archivo local como objeto S3

    aws s3 cp backup.tar s3://bucket-name
    upload: ./backup.tar to s3://bucket-name/backup.tar

Eliminar el objeto S3

    aws s3 rm s3://bucket-name/secret-file.gz .
    delete: s3://bucket-name/secret-file.gz

Descargar el bucket a local

    aws s3 sync s3://bucket-name/ /media/pasport-ultra/backup

Subir el directorio local al Bucket

    aws s3 sync (directory) s3://bucket-name/

Compartir el objeto S3 sin acceso público

    aws s3 presign s3://bucket-name/file-name --expires-in (time value)
    https://bucket-name.s3.amazonaws.com/file-name.pdf?AWSAccessKeyId=(key)&Expires=(value)&Signature=(value)

Se puede mezclar con un script de Linux. Por ejemplo, para subir todos los ficheros del directorio local:
```
for i in *
    do
        aws s3 cp $i s3://vergara-bucket/Docs/
    done
```

Una alternativa es el subcomando `s3api` que quizá tenga una sintaxis más parecida a lo general en AWS CLI

listar los buckets: 

	aws s3api list-buckets

Para crear un bucket:

	aws s3api create-bucket --bucket nombre-del-nuevo-bucket --region eu-west-3 --create-bucket-configuration LocationConstraint=eu-west-3

CUIDADO: Si no ponemos la region lo creará en us-east-1. Y si no ponemos la configuración del bucket no permite crearlo.

Para subir un objeto:

	aws s3api put-object --bucket nombre-bucket --key nombre-fichero-en-destino –body nombre-fichero-actual

Para listar objetos:

	aws s3api list-objects --bucket nombre-bucket

Para el plano de control podemos utilizar `s3control`

Para las operaciones de S3 on Outposts `s3outposts`


SNS
---

Lista de topics SNS

    aws sqs list-queues | jq -r ‘.QueueUrls[ ]’

Lista de topic SNS y suscripciones relacionadas

    aws sns list-subscriptions | jq -r '.Subscriptions[ ] | .TopicArn+" "+.Protocol+" "+.Endpoint'

Publicar en el topic SNS

    aws sns publish --topic-arn arn:aws:sns:ap-southeast-1:232398:backend-api-monitoring
    
Listar las suscripciones

	aws sns list-subscriptions

Crear un topic

	aws sns create-topic --name Nombre-del-topic

Asociar suscripción con un topic

	aws sns subscribe --topic-arn ARN-del-topic --protocol [http | https | email | email-json | sms | sqs | application | lambda | firehose] --notification-endpoint Correo-electronico-o-telefono-o…

Desasociar suscripción de un topic

	aws sns unsubscribe -subscription-arn ARN-de-suscripción

Borrar Topic

	aws sns delete-topic --topic-arn Nombre-ARN-del-topic

Borrar endpoint

	aws sns delete-endpoint



SQS
---

Listar queues

    aws sqs list-queues | jq -r '.QueueUrls[ ]'

Crear queue

    aws sqs create-queue --queue-name public-events.fifo | jq -r .queueURL

Enviar mensaje

    aws sqs send-message --queue-url (url) --message-body (mensaje)

Recibir mensaje

    aws sqs receive-message --queue-url (url) | jq -r '.Messages[ ] | .Body'

Borrar mensaje

    aws sqs delete-message --queue url (url) --receipt-handle (receipt handle)

Purgar la cola

    aws sqs purge-queue --queue-url (url)

Borrar cola

    aws sqs delete-queue --queue-url (url)


Filtros de las filas
--------------------

Filtrar según los datos que queremos mostrar. Ejemplos de opciones del comando ec2:

`--filters <value>`		Filtrar elementos (ver opciones: https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/describe-subnets.html#options) Se debe seguir las opciones de la doc. Ejemplo sintaxis general: 

	aws ec2 describe-subnets --filters "Name=state, Values=available" "Name=vpc-id, Values=vpc-06efefd12f7a7228f"

`[--subnet-ids <value>]`		Mostrar una subnet o varias por Ids

`[--dry-run | --no-dry-run]`	

`[--cli-input-json | --cli-input-yaml]`	

`[--starting-token <value>]`		A partir de qué elemento se empieza a listar

`[--page-size <value>]`			Determinar el tamaño de la página

`[--max-items <value>]`		Máximo de elementos a visualizar

`[--generate-cli-skeleton <value>]`	


Filtros de columnas. Opción Query
---------------------------------

Filtrar según los metadatos que queremos mostrar. El comando es `--query`. Ejemplos para filtrar subnets:

Filtrar subnets por array (Empieza en 0)

	aws ec2 describe-subnets --query 'Subnets[1]'

Filtrar por rango del array

	aws ec2 describe-subnets --query 'Subnets[1:3]'

Filtrar por key del array. El punto concatena el query 

	aws ec2 describe-subnets --query 'Subnets[*].AvailabilityZone'

Filtrar dos campos de dentro del array

	aws ec2 describe-subnets --query 'Subnets[].[SubnetId, CidrBlock]'

En VPC. Filtrar con tres columnas

	aws ec2 describe-vpcs --query 'Vpcs[].[VpcId,State,IsDefault]' --output table

Poner labels para identificar los campos

	aws ec2 describe-vpcs --query 'Vpcs[].[{"Id de VPC":VpcId},{Estado:State},IsDefault]'

Si se pone labels en todos los campos se puede mostrar como tabla:

	aws ec2 describe-vpcs --query 'Vpcs[].[{"Id de VPC":VpcId},{Estado:State},{"VPC por defecto":IsDefault}]' --output table


[Natxo Varona - AWS DevOps](https://nvarona.x10.bz)