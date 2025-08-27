# Como Executar o Serviço de Transcrição em uma VPS

Este guia explica como implantar e executar o serviço de transcrição de áudio `adfastltda/transcript` em um servidor privado virtual (VPS) usando Docker.

---
## Pré-requisitos

Antes de começar, certifique-se de que você tem o seguinte instalado em sua VPS:

*   **Docker:** [Instruções de instalação](https://docs.docker.com/engine/install/)
*   **Docker Compose:** [Instruções de instalação](https://docs.docker.com/compose/install/) (geralmente vem com o Docker Engine)

---
## Passo 1: Crie o arquivo `docker-compose.yml`

Crie um arquivo com o nome `docker-compose.yml` em um diretório de sua escolha na VPS e cole o seguinte conteúdo nele:

```yaml
version: '3.10'
services:
  transcript:
    image: adfastltda/transcript:latest
    ports:
      - "3434:3434"
    restart: always
```

**O que este arquivo faz?**
*   `image: adfastltda/transcript:latest`: Baixa a imagem de transcrição mais recente do Docker;
*   `ports: - "3434:3434"`: Expõe o serviço na porta 3434 da sua VPS;
*   `restart: always`: Garante que o contêiner reinicie automaticamente se ele parar por algum motivo.

## Passo 2: Inicie o Serviço

No mesmo diretório onde você criou o `docker-compose.yml`, execute o seguinte comando para baixar a imagem e iniciar o serviço:

```bash
docker compose up -d
```

O `-d` executa o serviço em modo "detached" (em segundo plano).

---
## Passo 3: Como Usar

O serviço de transcrição estará escutando na porta `3434` do IP da sua VPS. Para usar, envie uma requisição `POST` para o endpoint `http://<IP_DA_SUA_VPS>:3434/` com um corpo JSON contendo o áudio em base64:

**Exemplo de requisição (usando cURL):**
```bash
curl -X POST \
  http://0.0.0.0:3434/ \
  -H 'Content-Type: application/json' \
  -d '{
    "audio_base64": "...",
    "format": "ogg" (opcional)
  }'
```
Substitua `0.0.0.0` pelo endereço de IP do seu servidor, `...` pelo seu áudio codificado em base64 e `ogg` pelo formato do áudio. Se você não enviar o campo "format", o sistema continuará assumindo o formato "ogg" como padrão.

## Passo 4: Verificando os Logs

Para ver os logs do serviço em execução, use o comando:
```bash
docker compose logs
```


---
##### ☕ Buy Me a Coffee

Se você gosta deste projeto e quer apoiar o desenvolvimento, pode contribuir com um café!  

**PIX:** `pix@adfastltda.com.br`
**PayPal:** [Doar](https://www.paypal.com/ncp/payment/TSLA567NR39LA) 

Toda ajuda e sugestoes é bem-vinda e motivadora!  

Obrigado pelo apoio! 🙏

Para parar o serviço e remover o contêiner, execute:
```bash
docker compose down
```
