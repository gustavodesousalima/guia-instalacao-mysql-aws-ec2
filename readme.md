# Guia de Instalação do MySQL em Docker na AWS EC2 (Ubuntu 22)

Este documento fornece um passo a passo para criar uma instância EC2 na AWS, instalar o Docker e configurar o MySQL usando um container Docker. Esse setup permitirá que você acesse o banco de dados de uma API local.

## Pré-requisitos

- Conta na AWS
- Acesso ao terminal (Linux, macOS ou Windows com WSL)

## Passo 1: Criar uma Instância EC2

1. **Acesse o Console da AWS:**
   - Vá para [AWS Management Console](https://aws.amazon.com/console/) e faça login.

2. **Acesse o EC2:**
   - Procure por "EC2" e clique para abrir o serviço.

3. **Iniciar uma Nova Instância:**
   - Clique em “Launch Instance”.
   - Escolha uma Amazon Machine Image (AMI) (sugestão: **Ubuntu Server 22.04 LTS**).

4. **Escolher Tipo de Instância:**
   - Selecione um tipo de instância (ex: `t2.micro` para uso gratuito).

5. **Configurações de Instância:**
   - Clique em "Next: Configure Instance Details" e ajuste as configurações conforme necessário.

6. **Adicionar Armazenamento:**
   - Clique em "Next: Add Storage". O armazenamento padrão deve ser suficiente.

7. **Adicionar Tags (opcional):**
   - Adicione tags para identificar sua instância.

8. **Configurar Grupo de Segurança:**
   - Clique em "Next: Configure Security Group".
   - Adicione as seguintes regras:
     - **Tipo:** SSH | **Protocolo:** TCP | **Porta:** 22 | **Origem:** Seu IP (ou `0.0.0.0/0` para acesso público, não recomendado).
     - **Tipo:** Custom TCP | **Protocolo:** TCP | **Porta:** 3306 | **Origem:** Seu IP.

9. **Revisar e Lançar:**
   - Clique em “Review and Launch”, revise as configurações e clique em “Launch”.
   - Você precisará de uma chave SSH. Se não tiver, crie uma nova e faça o download.

10. **Conectar à Instância:**
    - Depois de lançada, selecione a instância e clique em “Connect” para ver as instruções de conexão via SSH. Use o terminal para conectar:
      
      ssh -i /caminho/para/sua-chave.pem ubuntu@<IP-da-sua-instância>
      

## Passo 2: Instalar o Docker

1. **Atualizar o Sistema:**

sudo apt update && sudo apt upgrade -y

## Instalar o Docker:

sudo apt install docker.io -y

## Iniciar o Docker e Adicionar ao Início:

sudo systemctl start docker
sudo systemctl enable docker

## Adicionar seu Usuário ao Grupo Docker (opcional):

sudo usermod -aG docker $USER
(Saia e entre novamente para aplicar as alterações de grupo.)

# Passo 3: Executar o MySQL Usando Docker

## Puxar a Imagem do MySQL:

docker pull mysql:latest

## Executar o Container do MySQL:

docker run --name meu-mysql -e MYSQL_ROOT_PASSWORD=sua-senha -p 3306:3306 -d mysql:latest

## Verificar se o Container está Rodando:

docker ps

# Passo 4: Acessar o MySQL

## Acessar o MySQL:

docker exec -it meu-mysql mysql -u root -p

## Criar um Banco de Dados:

sql
Copiar código
CREATE DATABASE nome_do_banco;

# Passo 5: Conectar sua API ao MySQL

## Utilize as credenciais que você configurou:

Usuário: root
Senha: sua-senha
Host: <IP-da-sua-instância>
Porta: 3306