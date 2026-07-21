# cURL - Um comando. Inúmeras possibilidades.

<div>
<img src="./images/curl-transparent.png">
</div>

Guia completo para utilizar o **cURL** em testes de APIs REST.

## Índice

- [1. Introdução ao cURL](#1-introdução-ao-curl)
    - [O que é o cURL](#o-que-é-curl)
    - [Por que desenvolvedores gostam do cURL?](#por-que-desenvolvedores-gostam-do-curl)

- [2. Fundamentos de Requisições HTTP](#2-fundamentos-de-requisições-http)
    - [Estrutura básica de uma requisição HTTP](#estrutura-básica-de-uma-requisição-http)
    - [Sintaxe Básica](#sintaxe-básica)

- [3. Trabalhando com Métodos HTTP](#3-trabalhando-com-metodos-http)
    - [GET - Consultando recursos](#get-consultando-recursos)
    - [POST - Criando um recurso](#post---criando-um-recurso)
    - [PUT - Atualizando um recurso](#put---atualizando-recursos-completos)
    - [PATCH - Atualizando parte do recurso](#patch---atualizações-parciais)
    - [DELETE - Removendo um recurso](#delete---removendo-recursos)

- [4. Headers e Parâmetros](#4-headers-e-parâmetros)
    - [Adicionando Cabeçalhos (Headers)](#adicionando-cabeçalhos-headers)
    - [Usando Query Parameters](#usando-query-parameters)

- [5. Autenticação](#5-autenticação)
    - [Login](#login)
    - [Basic Authentication](#basic-authentication)
    - [Bearer Token Authentication](#bearer-token-authentication)
    - [Salvando Token em variável de ambiente](#salvando-token-em-variável-de-ambiente)

- [6. Trabalhando com Arquivos](#6-trabalhando-com-arquivos)
    - [Salvando a resposta em um arquivo](#salvando-a-resposta-de-uma-consulta-em-um-arquivo)
    - [Download de arquivos](#fazendo-download-de-um-arquivo)
    - [Upload de arquivos](#fazendo-upload-de-um-arquivo)

- [7. Debug e Troubleshooting](#7-debug-e-troubleshooting)
    - [Visualizando apenas os Cabeçalhos](#visualizando-apenas-os-cabeçalhos-headers-da-resposta)
    - [Visualizando Headers e Body](#visualizando-headers-e-body)
    - [Seguindo Redirects](#seguindo-os-redirects)
    - [Modo Verbose](#modo-verbose)
    - [Ignorando erros de Certificados SSL](#ignorando-erros-de-certificados-ssl)

- [8. Cookies](#8-cookies) 
    - [Enviando Cookies](#enviando-cookies)
    - [Salvando Cookies](#salvando-cookies)

- [9. Comandos avançados](#9-comandos-avançados)
    - [Retornando o Status Code](#retornando-o-status-code)

- [10. Referência rápida](#10-referência-rápida)

- [11. Comparação](#11-comparação)

- [12. Boas práticas](#12-boas-práticas)

- [13. Pratique](#13-pratique)

# 1. Introdução ao cURL

Se você pretende trabalhar com APIs, aprender **cURL** é um dos primeiros passos.

Grande parte das documentações apresenta exemplos em cURL antes mesmo de mostrar exemplos em linguagens como Java, Python ou JavaScript. Isso acontece porque ele é simples, universal e está disponível na maioria dos sistemas operacionais.

Neste guia você aprenderá os principais comandos utilizados no dia a dia para testar APIs.

## O que é cURL

- Definição
O cURL (Client URL) é uma ferramenta de linha de comando utilizada para transferir dados entre um cliente e um servidor por meio de URLs.

- Protocolos suportados
Ela suporta dezenas de protocolos diferentes, sendo os mais utilizados para testes de APIs:

- HTTP
- HTTPS

Além deles, também oferece suporte para FTP, SFTP, SMTP, IMAP, LDAP, SCP, SMB, MQTT e vários outros protocolos.

## Por que desenvolvedores gostam do cURL?

Ao contrário de um cliente de API com interface gráfica, o cURL é:

- Leve
- Rápido
- Multiplataforma
- Scriptável
- Disponível por padrão no Linux e no macOS
- Integrado em versões modernas do Windows

É perfeito para:

- Teste de APIs REST
- Debugar respostas de servidor
- Automação de requisições para APIs
- Testar endpoints
- Download de arquivos
- Upload de arquivos
- Requisições autenticadas

# 2. Fundamentos de Requisições HTTP

Uma requisição HTTP é uma mensagem enviada por um cliente (como navegador, aplicativo mobile ou outro sistema) para um servidor solicitando uma ação ou informação. O servidor processa a solicitação e devolve uma resposta HTTP.

HTTP significa HyperText Transfer Protocol e é o principal protocolo usado para comunicação na Web e em APIs.

## Estrutura básica de uma requisição HTTP

Uma requisição HTTP possui principalmente:

1. Método HTTP — define a ação desejada.
2. URL — indica o recurso que será acessado.
3. Headers (cabeçalhos) — informações adicionais sobre a requisição.
4. Body (corpo) — dados enviados ao servidor (quando necessário).

## Sintaxe Básica

Enviando uma requisição com `GET`.

```bash
curl [opções] URL
```

# 3. Trabalhando com Métodos HTTP

## GET - Consultando recursos

Exemplo:

- Buscar usuários
- Retorno JSON

```bash
curl https://serverest.dev/usuarios
```

Esta requisição retorna dados de uma API:

```json
{
    "quantidade": 2,
    "usuarios": [
        {
            "nome": "Fulano da Silva",
            "email": "fulano@qa.com",
            "password": "teste",
            "administrador": "true",
            "_id": "0uxuPY0cbmQhpEz1"
        },
        {
            "nome": "Dough",
            "email": "dough@teste.com",
            "password": "testeemaisteste",
            "administrador": "false",
            "_id": "tcqN3BgfvPXT6k64"
        }
    ]
}
```

## POST - Criando um recurso

O método **POST** é utilizado para criar novos recursos em uma API.

- Enviando JSON
- Content-Type
- Status Code 201

Exemplo:

```bash
curl -X 'POST' \
  'https://serverest.dev/usuarios' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "nome": "Hora do qa",
  "email": "horadoqa@qa.com.br",
  "password": "1q2w3e4r",
  "administrador": "true"
}'
```

Onde:

- `-X` define o método HTTP.
- `-H` adiciona um cabeçalho (Headers).
- `-d` envia os dados da requisição (Body).

APIs modernas aceitam JSON

```json
'{
  "nome": "Hora do qa",
  "email": "horadoqa@qa.com.br",
  "password": "123456",
  "administrador": "true"
}'
```

No caso de sucesso, a resposta da API será um Código de Status 201 com a mensagem:

```json
{
    "message": "Cadastro realizado com sucesso",
    "_id": "zZtROwfJVSugF4Mw"
}
```

## PUT - Atualizando recursos completos

- Substituição do recurso
- Envio completo do JSON

O `PUT` atualiza recursos, no exemplo abaixo, alterando o `password`

```bash
curl -X 'PUT' \
  'https://serverest.dev/usuarios/zZtROwfJVSugF4Mw' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "nome": "Hora do qa",
  "email": "horadoqa@qa.com.br",
  "password": "1q2w3e4r",
  "administrador": "true"
}'
```

> **Importante:** em APIs REST tradicionais, o método `PUT` substitui completamente o recurso. Por isso, normalmente é necessário enviar todo o objeto JSON.

## PATCH - Atualizações parciais

- Diferença entre PUT e PATCH
- Quando utilizar

Diferentemente do `PUT`, o método `PATCH` permite atualizar apenas os campos desejados.
Usar quando queremos alterar/modificar somente um parâmetro.


```bash
curl -X 'PATCH' \
  'https://serverest.dev/usuarios/zZtROwfJVSugF4Mw' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{"password":"1q2w3e4r"}'
```

OBS.: O serverest.dev não permite a utilização do `PATCH`

Ao tentar...

```json
{
    "message": "Não é possível realizar PATCH em /usuarios/tcqN3BgfvPXT6k64. Acesse https://serverest.dev para ver as rotas disponíveis e como utilizá-las."
}  
```

## DELETE - Removendo recursos

- Exclusão de dados
- Respostas esperadas

A requisição `DELETE` removerá um recurso existente.

```bash
curl -X 'DELETE' 'https://serverest.dev/usuarios/zZtROwfJVSugF4Mw' \
    -H 'accept: application/json'
```

Ao executar o comando, a API nos retornará uma mensagem:

```json
{
    "message": "Registro excluído com sucesso"
}
```

# 4. Headers e Parâmetros

Os Cabeçalhos (Headers) enviam informações adicionais para a API, como:

- Authorization
- Content-Type
- Accept
- User-Agent

## Usando Query Parameters

- Filtros
- Busca
- Paginação

Os Query Parameters são adicionados após o caractere `?` e permitem filtrar, pesquisar ou paginar resultados.

No **ServeRest**, os **Query Parameters** são usados principalmente para filtrar recursos em endpoints como `/usuarios`, `/produtos` e `/carrinhos`.

### Exemplo 1: Buscar usuário por nome

```http
GET https://serverest.dev/usuarios?nome=Fulano%20da%20Silva
```

Ou com `curl`:

```bash
curl -X GET "https://serverest.dev/usuarios?nome=Fulano%20da%20Silva"
```

---

### Exemplo 2: Buscar usuário por e-mail

```http
GET https://serverest.dev/usuarios?email=fulano@email.com
```

```bash
curl -X GET "https://serverest.dev/usuarios?email=fulano@email.com"
```

---

### Exemplo 3: Buscar usuários administradores

```http
GET https://serverest.dev/usuarios?administrador=true
```

```bash
curl -X GET "https://serverest.dev/usuarios?administrador=true"
```

---

### Exemplo 4: Combinar vários Query Parameters

```http
GET https://serverest.dev/usuarios?nome=Fulano%20da%20Silva&administrador=true
```

```bash
curl -X GET "https://serverest.dev/usuarios?nome=Fulano%20da%20Silva&administrador=true"
```

Nesse caso, a API retorna usuários que atendam aos dois critérios.

---

### Exemplo 5: Produtos

```http
GET https://serverest.dev/produtos?nome=Logitech MX Vertical
```

ou

```bash
curl -X GET "https://serverest.dev/produtos?nome=Logitech%20MX%20Vertical"
```

---

### Estrutura geral

A URL segue o padrão:

```text
https://serverest.dev/recurso?campo1=valor1&campo2=valor2
```

Por exemplo:

```text
GET /usuarios?email=teste@email.com&administrador=false
```

Os parâmetros (`email`, `administrador`, `nome`, etc.) correspondem aos campos do recurso e permitem filtrar os resultados retornados pela API.

# 5. Autenticação

## Login

Podemos realizar o login na API também utilizando o `POST`

```bash
curl -X 'POST' 'https://serverest.dev/login' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "email": "horadoqa@qa.com.br",
  "password": "1q2w3e4r"
}'
```

Que retornará:

```json
{
    "message": "Login realizado com sucesso",
    "authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6Im"
}
```

## Basic Authentication

```bash
curl -u usuario:senha URL
```

Equivalente ao enviar: `Authorization: Basic...`

## Bearer Token Authentication

- O que é um Bearer Token
- Enviando Token manualmente

Um Bearer Token é um tipo de token de autenticação usado para provar que um cliente está autorizado a acessar um recurso (como uma API).

```bash
curl -X POST "https://serverest.dev/produtos" \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6I" \
  -d '{
    "nome": "Logitech MX Vertical",
    "preco": 470,
    "descricao": "Mouse",
    "quantidade": 381
  }'
```

Neste caso, o Token fica exposto, mas podemos usar a variável de ambiente:

```bash
curl -X POST "https://serverest.dev/produtos" \
  -H "Authorization: $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "nome": "Produto Teste",
    "preco": 100,
    "descricao": "Exemplo",
    "quantidade": 10
  }'
```

A resposta esperada é:

```json
{
    "message": "Cadastro realizado com sucesso",
    "_id": "OSGFEloR3uPp3mra"
}
```

## Salvando Token em variável de ambiente
- Login
- Extração usando jq
- Reutilização em chamadas futuras

Exemplo:

```bash
TOKEN=$(curl ... | jq -r '.authorization')
```

Após realizar o login, podemos armazenar o token retornado pela API em uma variável de ambiente para reutilizá-lo nas próximas requisições, desse modo o token não fica exposto.

Utilizamos o [./jq](https://jqlang.org/) que é um processador de JSON que facilita a leitura da resposta da API.

```bash
TOKEN=$(curl -s -X POST "https://serverest.dev/login" \
  -H "accept: application/json" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "horadoqa@qa.com.br",
    "password": "1q2w3e4r"
  }' | jq -r '.authorization')
```

Para verificar o conteúdo da variável:

```bash
echo $TOKEN
```

# 6. Trabalhando com Arquivos

## Salvando a resposta de uma consulta em um arquivo

Podemos salvar a resposta da consulta dos usuários em um arquivo: `user.json`

```bash
curl -o arquivo.json URL
```

## Fazendo Download de um arquivo

Neste exemplo, será feito o download de uma imagem.

Exemplo:

```bash
curl -O URL
```

## Upload de arquivos

Neste exemplo, enviando uma imagem

```bash
curl -F "file=@arquivo.jpg" URL
```

> OBS.: Para que este comando funcione, a API precisa aceitar.

# 7. Debug e Troubleshooting

## Visualizando apenas os Cabeçalhos (Headers) da resposta

```bash
curl -I https://serverest.dev/
```

Que retornará:

```bash
HTTP/2 200
access-control-allow-origin: *
x-dns-prefetch-control: off
x-frame-options: SAMEORIGIN
strict-transport-security: max-age=15552000; includeSubDomains
x-download-options: noopen
x-content-type-options: nosniff
x-xss-protection: 1; mode=block
cache-control: no-cache, max-age=0
content-type: text/html; charset=utf-8
x-cloud-trace-context: 2321af5d96783c913119b5329d248ffc;o=1
content-length: 33114
date: Mon, 20 Jul 2026 23:32:00 GMT
server: Google Frontend
```

## Visualizando Headers e Body

```bash
curl -i https://serverest.dev/
```

Retornará os Headers e todo o Body da resposta.

## Seguindo os Redirects

```bash
curl -L URL
```

## Modo Verbose

```bash
curl -v URL
```

O modo Verbose exibe informações detalhadas da comunicação HTTP:

- Headers da requisição
- Headers da resposta
- Negociação SSL/TLS
- Informações da conexão
- Redirecionamentos

```bash
*   Trying 216.239.34.21:443...
* Connected to serverest.dev (216.239.34.21) port 443 (#0)
* ALPN, offering h2
* ALPN, offering http/1.1
*  CAfile: /etc/ssl/certs/ca-certificates.crt
*  CApath: /etc/ssl/certs
* TLSv1.0 (OUT), TLS header, Certificate Status (22):
* TLSv1.3 (OUT), TLS handshake, Client hello (1):
* TLSv1.2 (IN), TLS header, Certificate Status (22):
* TLSv1.3 (IN), TLS handshake, Server hello (2):
* TLSv1.2 (IN), TLS header, Finished (20):
* TLSv1.2 (IN), TLS header, Supplemental data (23):
* TLSv1.3 (IN), TLS handshake, Encrypted Extensions (8):
* TLSv1.3 (IN), TLS handshake, Certificate (11):
* TLSv1.3 (IN), TLS handshake, CERT verify (15):
* TLSv1.3 (IN), TLS handshake, Finished (20):
* TLSv1.2 (OUT), TLS header, Finished (20):
* TLSv1.3 (OUT), TLS change cipher, Change cipher spec (1):
* TLSv1.2 (OUT), TLS header, Supplemental data (23):
* TLSv1.3 (OUT), TLS handshake, Finished (20):
* SSL connection using TLSv1.3 / TLS_AES_256_GCM_SHA384
* ALPN, server accepted to use h2
* Server certificate:
*  subject: CN=serverest.dev
*  start date: Jun 17 12:02:02 2026 GMT
*  expire date: Sep 15 12:53:55 2026 GMT
*  subjectAltName: host "serverest.dev" matched cert's "serverest.dev"
*  issuer: C=US; O=Google Trust Services; CN=WR3
*  SSL certificate verify ok.
* Using HTTP2, server supports multiplexing
* Connection state changed (HTTP/2 confirmed)
* Copying HTTP/2 data in stream buffer to connection buffer after upgrade: len=0
* TLSv1.2 (OUT), TLS header, Supplemental data (23):
* TLSv1.2 (OUT), TLS header, Supplemental data (23):
* TLSv1.2 (OUT), TLS header, Supplemental data (23):
* Using Stream ID: 1 (easy handle 0x560d4815aa40)
* TLSv1.2 (OUT), TLS header, Supplemental data (23):
> GET /usuarios HTTP/2
> Host: serverest.dev
> user-agent: curl/7.81.0
> accept: */*
>
* TLSv1.2 (IN), TLS header, Supplemental data (23):
* TLSv1.2 (OUT), TLS header, Supplemental data (23):
* TLSv1.2 (IN), TLS header, Supplemental data (23):
* TLSv1.2 (IN), TLS header, Supplemental data (23):
< HTTP/2 200
< access-control-allow-origin: *
< x-dns-prefetch-control: off
< x-frame-options: SAMEORIGIN
< strict-transport-security: max-age=15552000; includeSubDomains
< x-download-options: noopen
< x-content-type-options: nosniff
< x-xss-protection: 1; mode=block
< content-type: application/json; charset=utf-8
< x-cloud-trace-context: feefbcfb787c7486686dc32ae2164e95;o=1
< date: Mon, 20 Jul 2026 23:38:09 GMT
< server: Google Frontend
< content-length: 144211
<
* TLSv1.2 (IN), TLS header, Supplemental data (23):
```

## Ignorando erros de Certificados SSL

```bash
curl -k URL
```

> Evite utilizar essa opção em ambientes de produção, pois ela desabilita a validação do certificado SSL.

# 8. Cookies

## Enviando Cookies

```bash
curl -b cookies.txt URL
```

## Salvando Cookies

```bash
curl -c cookies.txt URL
```

# 9. Comandos avançados

## Retornando o Status Code

```bash
curl -o /dev/null -s -w "%{http_code}\n" URL
```

Terá como resposta: 

```bash
200
```

# 10. Referência rápida

## Principais opções do cURL

| Opção | Descrição                    |
| ----- | ---------------------------- |
| `-X`  | Define o método HTTP         |
| `-H`  | Adiciona um Header           |
| `-d`  | Envia dados                  |
| `-i`  | Exibe Headers + Body         |
| `-I`  | Exibe apenas os Headers      |
| `-v`  | Modo Verbose                 |
| `-L`  | Segue Redirects              |
| `-o`  | Salva a resposta em arquivo  |
| `-O`  | Salva usando o nome original |
| `-u`  | Basic Authentication         |
| `-k`  | Ignora erros SSL             |
| `-b`  | Envia Cookies                |
| `-c`  | Salva Cookies                |
| `-F`  | Envia formulário/multipart   |

# 11. Comparação

## cURL versus Postman

| Recurso              | cURL  | Postman |
| -------------------- | ----- | ------- |
| Linha de comando     | ✅     | ❌       |
| Interface gráfica    | ❌     | ✅       |
| Versionamento (Git)  | ⭐⭐⭐⭐⭐ | ⭐⭐      |
| Automação            | ⭐⭐⭐⭐⭐ | ⭐⭐⭐     |
| Integração com CI/CD | ⭐⭐⭐⭐⭐ | ⭐⭐⭐     |
| Scripts              | ⭐⭐⭐⭐⭐ | ⭐⭐⭐     |
| Consumo de memória   | Baixo | Alto    |
| Curva de aprendizado | Média | Baixa   |


# 12. Boas práticas

- Utilize HTTPS sempre que possível.
- Nunca exponha tokens ou chaves de API no código-fonte.
- Armazene credenciais em variáveis de ambiente.
- Utilize `-v` para depuração de problemas.
- Sempre valide o Status Code e o corpo da resposta.
- Automatize comandos repetitivos utilizando Shell Script.
- Prefira `PATCH` para alterações parciais, quando a API oferecer suporte.
- Evite utilizar `-k` em ambientes de produção.
- Usar o jq que é um processador de JSON que facilita a leitura da resposta da API.


# 13. Pratique

Execute o fluxo completo de teste de API usando o [Serverest.dev](https://serverest.dev).

1. Cadastrar um usuário com a flag: "administrador": "false"

2. Verificar se o usuário está na lista de usuários

3. Alterar a flag para: "administrador": "true"

4. Fazer login

5. Capturar token e salvar em uma variável de ambiente

6. Cadastrar um produto usando a variável.

7. Verificar se o produto está na lista de produtos

Bons Estudos !!!