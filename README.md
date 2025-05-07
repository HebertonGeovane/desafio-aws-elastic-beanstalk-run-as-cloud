# 🚀 desafio-aws-elastic-beanstalk-run-as-cloud

Neste laboratório, vamos implantar uma aplicação de exemplo no **AWS Elastic Beanstalk**, utilizando **recursos elegíveis ao Free Tier da AWS**. Essa atividade é parte do grupo de estudos **Run as Cloud** e tem como objetivo te guiar na prática sobre como esse serviço gerencia automaticamente **instâncias EC2, balanceadores de carga, grupos de segurança e escalabilidade**.

---

## 🔐 Etapa Inicial – Criar Role IAM para Elastic Beanstalk

Antes de criar o ambiente Elastic Beanstalk, é necessário criar uma role IAM com permissões apropriadas.

### 📌 Nome da role
`aws-elasticbeanstalk-ec2-role`

### 📄 Descrição
Permite que instâncias EC2 criadas pelo Elastic Beanstalk chamem serviços AWS em seu nome.



### 🔐 Políticas anexadas

| Política                             | Tipo            | Descrição                             |
|--------------------------------------|------------------|----------------------------------------|
| `AmazonEC2FullAccess`                | AWS gerenciada  | Acesso total a EC2                     |
| `AWSElasticBeanstalkEnhancedHealth`  | AWS gerenciada  | Monitoramento de saúde                 |
| `AWSElasticBeanstalkService`         | AWS gerenciada  | Permissões básicas do serviço Beanstalk|
| `AWSElasticBeanstalkWebTier`         | AWS gerenciada  | Recursos de front-end para Beanstalk   |
| `AWSElasticBeanstalkWorkerTier`      | AWS gerenciada  | Suporte a aplicações de backend        |
| `AWSSupportAccess`                   | AWS gerenciada  | Acesso aos recursos do AWS Support     |




> **Observação**: essa role deve ser associada automaticamente pelo Elastic Beanstalk ao criar seu ambiente, mas é importante garantir que ela exista previamente.

📸 **Print obrigatório:**

 Console IAM mostrando a role criada: `aws-elasticbeanstalk-ec2-role`

 Aba Policies da role com as 6 permissões anexadas

![image (95)](https://github.com/user-attachments/assets/7fd35848-ae79-4285-8c12-be97958a5e51)


---

## ⚠️ Atenção antes de começar

Embora este laboratório utilize recursos dentro da camada gratuita da AWS, **é fundamental acompanhar o uso**:

- Monitore o uso no **AWS Billing Dashboard** e no **AWS Cost Explorer**
- **Encerre o ambiente Elastic Beanstalk ao final da atividade**
- **Evite publicar prints com dados sensíveis** (como IDs de conta ou URLs privadas)

---

## 📚 Objetivo

Implantar uma aplicação Java (Tomcat) de exemplo no AWS Elastic Beanstalk, observando a criação automática dos recursos de infraestrutura:

- Instância EC2
- Load Balancer
- Auto Scaling
- Grupo de Segurança
- Regras de rede

---

## 🌐 O que é o Elastic Beanstalk?

O **AWS Elastic Beanstalk** é um serviço de PaaS (Platform as a Service) que facilita o deploy e gerenciamento de aplicações. Com ele, você envia seu código e o Beanstalk cuida do provisionamento da infraestrutura, monitoramento, escalabilidade e atualizações automáticas.

---

## 🏗️ Etapas do laboratório

### 🟢 Etapa 1 – Baixar o aplicativo de exemplo

Baixe o aplicativo de exemplo fornecido pela AWS (Tomcat):

🔗 [Download do exemplo Tomcat](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/samples/tomcat.zip)

---

### 🟢 Etapa 2 – Criar ambiente no Elastic Beanstalk

No Console AWS:

1. Acesse **Elastic Beanstalk > Applications > Create application**
2. Preencha os dados:

- **Application name**: `run-as-cloud-elasticbeanstalk`
- **Platform**: `Tomcat`
- **Platform branch/version**: selecione a versão mais recente disponível
- **Application code**: Upload do `.zip` baixado anteriormente

📸 **Print obrigatório:**

 Tela de criação da application `run-as-cloud-elasticbeanstalk`
 
 
![image](https://github.com/user-attachments/assets/25cd7f79-3c7a-4004-8fa1-4d19b79f6d7c)



---

### 🟡 Etapa 3 – Configurar ambiente

- **Environment name**: `run-as-cloud-env` (de 4 a 40 caracteres, sem iniciar ou terminar com hífen)
- **Domain**: exemplo: `run-as-cloud-env.us-east-1.elasticbeanstalk.com`
- **Environment tier**: `Web Server`
 
📸 **Print obrigatório:**

Tela com:

Environment name preenchido: `run-as-cloud-env`

Subdomínio `.us-east-1.elasticbeanstalk.com`

Tier: `Web Server`

![image](https://github.com/user-attachments/assets/9f900406-5b3c-4cff-a705-cfcdcf1eea58)

---
### 🛠️ Configuração da Plataforma

Na etapa de definição da plataforma, preencha da seguinte forma:

| Campo              | Valor selecionado                                                        |
|--------------------|--------------------------------------------------------------------------|
| **Platform type**  | `Managed platform`                                                       |
| **Platform**       | `Tomcat`                                                                 |
| **Platform branch**| `Tomcat 11 with Corretto 21 running on 64bit Amazon Linux 2023`         |
| **Platform version**| `5.6.1 (Recommended)`                                                   |

✅ Essa configuração é baseada no uso de Java com Tomcat, e totalmente gerenciada pela AWS.

📸 **Print obrigatório:**
- Tela de seleção da plataforma com os campos preenchidos conforme acima
![image](https://github.com/user-attachments/assets/58c67c63-4cbc-4554-9804-27a3177c8ff3)

---

### 📦 Upload do Código da Aplicação

Nesta seção, você fará o upload do seu código-fonte `.zip`.

| Campo                  | Valor                            |
|------------------------|----------------------------------|
| **Application code**   | `Upload your code`               |
| **Source code origin** | `Local file`                     |
| **File name**          | `tomcat.zip`                     |
| **Version label**      | `tomcat-v1`                      |

> ⚠️ O campo `Version label` precisa ser **único** dentro da aplicação. Recomendamos seguir um padrão como `tomcat-v1-run-as-cloud`.

📸 **Print obrigatório:**
- Tela de upload do arquivo `tomcat.zip`
- Campo `Version label` preenchido com `tomcat-v1`

![image](https://github.com/user-attachments/assets/0e805285-b050-4ce2-b409-76c1211eaf3e)



---

### ⚙️ Escolha de Preset de Configuração

A AWS oferece configurações prontas (presets) para facilitar a criação do ambiente. Para esse laboratório, utilizamos:

| Campo                     | Valor selecionado                        |
|---------------------------|------------------------------------------|
| **Configuration presets** | `Single instance (free tier eligible)`   |

✅ Essa opção garante que o ambiente seja executado com **mínimos custos**, usando apenas uma instância `t2.micro` elegível ao Free Tier.

📸 **Print obrigatório:**
- Tela de seleção de `Configuration presets` com a opção `Single instance (free tier eligible)` marcada

![image](https://github.com/user-attachments/assets/7a3c0095-6cc5-4e26-b387-40b655b74ab6)

---


### ⚙️ Etapas avançadas (opcional)
Durante o provisionamento do ambiente, você verá uma sequência de etapas opcionais. Para simplificar e manter o foco no que é essencial (e gratuito), você pode optar por “Skip to review” e aceitar as configurações padrão, com exceção da configuração da role IAM e key pair.

#### 🔹 Configure service access

- Permita que a AWS crie uma role IAM automaticamente ou use a `aws-elasticbeanstalk-ec2-role`.

Campo	Valor selecionado

Service role	`Use an existing service role`

Selected role	`aws-elasticbeanstalk-ec2-role`

EC2 instance profile	`aws-elasticbeanstalk-ec2-role`

EC2 key pair	`EC2-Tutorial` ou `Proceed without key pair`

> ⚠️ Se você não precisa de acesso SSH à instância, pode optar por "Proceed without key pair".

📸 **Print obrigatório:**

- Tela com a role `aws-elasticbeanstalk-ec2-role` selecionada como Service role

- EC2 key pair selecionado ou opção “Proceed without key pair”

- EC2 instance profile selecionado com `aws-elasticbeanstalk-ec2-role`

  ![image](https://github.com/user-attachments/assets/f80d957c-0d05-4ee2-8e9f-46b7a518dbef)


---

### 🟢 Etapa 4 – Review e Criar

- Revise todas as configurações e clique em **Create environment**
- Aguarde o ambiente ser provisionado (pode levar alguns minutos)

![image](https://github.com/user-attachments/assets/af58fc5d-e610-4539-996a-3fa086b79445)



---

### 🌐 Etapa 5 – Acessar aplicação

1. Após a criação do ambiente, acesse a **URL pública** informada
2. Você verá a página de boas-vindas do exemplo Tomcat

![image](https://github.com/user-attachments/assets/7cf59277-ab2f-4ed7-8de1-3834e3aaaf63)

---

## 📸 Registrar Evidências

- Ambiente Elastic Beanstalk criado com sucesso
- URL pública acessando a aplicação
- Página Tomcat acessível no navegador

---

## ✅ Conclusão

Neste laboratório do desafio **AWS Elastic Beanstalk Run as Cloud**, você:

- Criou uma aplicação Elastic Beanstalk
- Fez upload de um app Java (Tomcat) de exemplo
- Observou a criação automatizada de instância EC2, balanceador e auto scaling
- Acessou a aplicação via navegador

---

## 🧹 Limpeza dos recursos

Após concluir o laboratório:

- Acesse o Elastic Beanstalk > Environment > **Terminate environment**
- Isso irá apagar automaticamente a instância, o load balancer e demais recursos associados

✅ Verifique no painel de faturamento que não há recursos ativos

---

## 📢 Compartilhe seu progresso!

Marque a comunidade **Run as Cloud** no LinkedIn:  
🔗 [Run as Cloud no LinkedIn](https://www.linkedin.com/company/run-as-cloud/?viewAsMember=true)

**Organizadores:**

- [Heberton Geovane](https://www.linkedin.com/in/heberton-geovane/)
- [Maik Biazi](https://www.linkedin.com/in/maik-biazi-47b9291a5/)
- [Michel Ernesto](https://www.linkedin.com/in/mernesto/)

---
