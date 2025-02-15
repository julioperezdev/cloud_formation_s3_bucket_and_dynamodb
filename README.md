# AWS CloudFormation - Creando y Eliminando un Bucket S3 y una Tabla DynamoDB

Este ejercicio muestra cómo usar **AWS CloudFormation** para crear y eliminar un **bucket S3** y una **tabla DynamoDB** de manera automatizada. Es un primer paso para aprender Infrastructure as Code (IaC) con AWS.

## 📌 Requisitos Previos
Antes de ejecutar este ejercicio, asegúrate de tener lo siguiente:
- Una cuenta de **AWS** activa.
- **AWS CLI** instalada y configurada con permisos adecuados.
- Un editor de texto (como VS Code) para escribir la plantilla YAML.

## 🚀 Paso a Paso

### 1️⃣ Crear la plantilla CloudFormation
Crea un archivo llamado `s3-dynamodb.yml` con el siguiente contenido:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
  
  MyDynamoDBTable:
    Type: AWS::DynamoDB::Table
    Properties:
      TableName: MyDynamoDBTable
      AttributeDefinitions:
        - AttributeName: id
          AttributeType: S
      KeySchema:
        - AttributeName: id
          KeyType: HASH
      ProvisionedThroughput:
        ReadCapacityUnits: 5
        WriteCapacityUnits: 5
```

Esta plantilla define dos recursos:
1. Un **bucket S3** en AWS.
2. Una **tabla DynamoDB** con una clave primaria (`id`).

### 2️⃣ Crear el Stack en AWS CloudFormation
Ejecuta el siguiente comando en la terminal para desplegar el Stack en AWS:

```sh
aws cloudformation create-stack --stack-name MyStorageStack --template-body file://s3-dynamodb.yml
```

Salida esperada:
```json
{
    "StackId": "arn:aws:cloudformation:region:account-id:stack/MyStorageStack/stack-id"
}
```

Para verificar el estado del Stack:
```sh
aws cloudformation describe-stacks --stack-name MyStorageStack
```

### 3️⃣ Verificar que los Recursos se Crearon
Puedes listar los buckets en tu cuenta con el siguiente comando:
```sh
aws s3 ls
```
Para ver la tabla DynamoDB creada:
```sh
aws dynamodb list-tables
```
Si la tabla **MyDynamoDBTable** aparece en la lista, se creó correctamente.

### 4️⃣ Eliminar el Stack y los Recursos
Para eliminar el Stack y sus recursos ejecuta:
```sh
aws cloudformation delete-stack --stack-name MyStorageStack
```

Para verificar la eliminación:
```sh
aws cloudformation describe-stacks --stack-name MyStorageStack
```
Si el Stack ya no existe, significa que se eliminó correctamente.

**⚠️ Nota:** Si el bucket tenía archivos dentro o la tabla tenía datos, CloudFormation **no los eliminará** automáticamente. En ese caso:

1️⃣ **Vacía el bucket manualmente**:
```sh
aws s3 rm s3://my-unique-bucket-name --recursive
aws s3api delete-bucket --bucket my-unique-bucket-name
```

2️⃣ **Elimina la tabla DynamoDB manualmente**:
```sh
aws dynamodb delete-table --table-name MyDynamoDBTable
```

---

## 🎯 ¿Qué aprendimos?
✔️ Cómo crear múltiples recursos en AWS con CloudFormation.  
✔️ Cómo desplegar un Stack desde la CLI.  
✔️ Cómo verificar y eliminar recursos en AWS.  

📌 **Siguientes pasos:** Puedes extender este ejercicio agregando más configuraciones, como políticas de acceso a S3 o índices globales en DynamoDB.

🚀 ¡Sigue explorando CloudFormation para desplegar infraestructuras más avanzadas! 🎯

