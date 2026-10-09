# 🚀 Primeiro Projeto DevOps | Docker + AWS EC2 + Amazon ECR

Este repositório documenta meu primeiro laboratório prático de **DevOps**, desenvolvido para aplicar conceitos de containerização, computação em nuvem, redes e implantação de aplicações.

O objetivo foi realizar o **deploy manual de uma aplicação web estática na AWS**, utilizando Docker para empacotar a aplicação, Amazon ECR para armazenar a imagem e Amazon EC2 para executar o contêiner.

> 📚 Projeto de estudos inspirado no tutorial **[Seu Primeiro Projeto Prático DevOps COMPLETO: Docker, AWS, Terraform e CI/CD!](https://www.youtube.com/watch?v=UEoxMU_l2xs)**, de Maria Lazara. Esta documentação descreve a **fase inicial** do laboratório; Terraform e CI/CD são possibilidades de evolução, não funcionalidades implementadas nesta fase.

## 🎯 Objetivos

- Compreender a criação e execução de imagens Docker.
- Publicar uma imagem de contêiner no Amazon ECR.
- Provisionar e acessar uma instância Amazon EC2.
- Executar uma aplicação web em um contêiner na nuvem.
- Entender como portas, regras de Security Group e conectividade afetam o acesso à aplicação.

## 🛠️ Tecnologias utilizadas

| Tecnologia | Finalidade |
| --- | --- |
| **Linux** | Ambiente de desenvolvimento e comandos de terminal |
| **Git e GitHub** | Versionamento e hospedagem do código |
| **Docker** | Construção de imagem e execução do contêiner |
| **AWS CLI** | Interação com os serviços AWS pelo terminal |
| **Amazon ECR** | Repositório de imagens Docker |
| **Amazon EC2** | Servidor virtual para executar a aplicação |
| **HTML, CSS e JavaScript** | Tecnologias da aplicação web estática |

## 🏗️ Arquitetura do laboratório

```text
Código da aplicação
        │
        ▼
  Docker build
        │
        ▼
  Imagem Docker
        │
        ▼
   Amazon ECR
        │
        │ docker pull
        ▼
   Amazon EC2
        │
        │ docker run
        ▼
  Contêiner web
        │
        ▼
Navegador do usuário
```

O acesso pelo navegador depende da publicação correta da porta do contêiner, das regras de rede e da disponibilidade da aplicação.

## 📋 Pré-requisitos

- Conta AWS com permissões adequadas para EC2 e ECR.
- Docker e AWS CLI instalados e configurados.
- Git instalado.
- Acesso SSH à instância EC2.
- Conhecimentos básicos de Linux e redes.

## ⚙️ Etapas do projeto

### 1. Construção da imagem Docker

Na pasta da aplicação, utilizando o `Dockerfile` correspondente:

```bash
docker build -t meu-site:latest .
```

Verificação da imagem criada:

```bash
docker images
```

### 2. Teste local do contêiner

Exemplo para uma aplicação cujo serviço escuta na porta 80 do contêiner:

```bash
docker run -d --name meu-site -p 8080:80 meu-site:latest
```

A aplicação poderá ser testada em `http://localhost:8080`, desde que o contêiner esteja em execução e o serviço esteja escutando na porta indicada.

### 3. Publicação da imagem no Amazon ECR

Depois de criar um repositório privado no ECR, autentique o Docker (substitua os valores de exemplo):

```bash
aws ecr get-login-password --region <REGIAO> | \
  docker login --username AWS --password-stdin \
  <ID_CONTA>.dkr.ecr.<REGIAO>.amazonaws.com
```

Marque e envie a imagem:

```bash
docker tag meu-site:latest <ID_CONTA>.dkr.ecr.<REGIAO>.amazonaws.com/meu-site:latest

docker push <ID_CONTA>.dkr.ecr.<REGIAO>.amazonaws.com/meu-site:latest
```

### 4. Deploy na Amazon EC2

Após provisionar a instância, instalar o Docker e configurar permissões de acesso ao ECR, autentique o Docker no registro, baixe a imagem e execute o contêiner:

```bash
docker pull <ID_CONTA>.dkr.ecr.<REGIAO>.amazonaws.com/meu-site:latest

docker run -d --name meu-site -p 80:80 \
  <ID_CONTA>.dkr.ecr.<REGIAO>.amazonaws.com/meu-site:latest
```

> Os comandos pressupõem que a aplicação escuta na porta 80 dentro do contêiner. Ajuste as portas se o seu `Dockerfile` usar outra configuração. Para acessar o ECR na EC2, prefira uma **IAM Role** com permissões mínimas necessárias, sem armazenar chaves AWS no código.

### 5. Validação e diagnóstico

```bash
docker ps
docker logs meu-site
sudo ss -tulnp
```

Caso a aplicação não abra pelo IP público da EC2, verifique:

- Se o contêiner está em execução.
- Se o mapeamento de portas está correto (`-p PORTA_HOST:PORTA_CONTAINER`).
- Se o Security Group permite tráfego HTTP na porta utilizada.
- Se a instância tem conectividade pública e roteamento adequado.
- Se o serviço web está escutando na interface e porta esperadas.

Para um laboratório, a porta 80 pode ser utilizada para HTTP. Em um ambiente de produção, considere HTTPS, restrições de acesso e outras medidas de segurança.

## 🧠 Aprendizados

Durante a execução do laboratório, pratiquei a criação e publicação de imagens Docker, o deploy manual na AWS e a análise de problemas de conectividade entre o navegador e a aplicação. A atividade também reforçou a importância de compreender **redes, portas e Security Groups**, além de apenas executar comandos de deploy.

## 🔜 Próximas evoluções

- [ ] Provisionar infraestrutura com **Terraform** (Infrastructure as Code).
- [ ] Automatizar build e deploy com **GitHub Actions**.
- [ ] Adicionar verificações de qualidade e segurança à pipeline.
- [ ] Melhorar observabilidade, documentação e tratamento de falhas.

## 📚 Referência

Laboratório baseado no conteúdo educacional de **Maria Lazara**:

▶️ [Seu Primeiro Projeto Prático DevOps COMPLETO: Docker, AWS, Terraform e CI/CD!](https://www.youtube.com/watch?v=UEoxMU_l2xs)

O repositório registra minha execução e meus aprendizados a partir do tutorial, com adaptações e documentação própria.

---

**Autora:** [Suelem Macedo](https://github.com/suelemmacedo)  
**Repositório:** [Primeiro-Projeto-DevOps](https://github.com/suelemmacedo/Primeiro-Projeto-DevOps)
