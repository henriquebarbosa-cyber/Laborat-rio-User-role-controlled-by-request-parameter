# PortSwigger Lab — User Role Controlled by Request Parameter
[site lab](https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter)

## Sobre o projeto

Este projeto documenta a resolução de um laboratório da PortSwigger Web Security Academy relacionado a controle de acesso.

O laboratório demonstra uma situação em que a aplicação utiliza um parâmetro controlado pelo cliente para determinar se o usuário possui privilégios administrativos.

Durante a prática, utilizei o Burp Suite para interceptar e analisar requisições HTTP, identificar o parâmetro relacionado ao privilégio administrativo e testar a alteração de seu valor.

> Este laboratório foi realizado exclusivamente no ambiente controlado disponibilizado pela PortSwigger Web Security Academy.

## Laboratório

**Nome:** User role controlled by request parameter

**Plataforma:** PortSwigger Web Security Academy

**Categoria:** Access Control

## Objetivo

Identificar uma falha de controle de acesso na qual o nível de privilégio do usuário pode ser alterado por meio da manipulação de um parâmetro presente na requisição.

O objetivo final do laboratório era acessar a área administrativa e excluir o usuário `carlos`.

## Ambiente utilizado

- PortSwigger Web Security Academy
- Burp Suite Community Edition
- Firefox
- Windows

## Conceito estudado

O laboratório apresenta uma falha relacionada à autorização.

Durante a análise, foi identificado que a aplicação utilizava o parâmetro:

    Admin=false

para indicar que o usuário não possuía privilégios administrativos.

Como esse valor era enviado pelo cliente, foi possível modificar a requisição antes de encaminhá-la ao servidor.

A alteração realizada foi:

    Admin=false

para:

    Admin=true

Essa modificação permitiu acessar funcionalidades administrativas da aplicação.

## Metodologia

### 1. Login

Inicialmente, realizei login no laboratório utilizando o usuário fornecido pela própria plataforma:

    Usuário: wiener

Após a autenticação, utilizei o Burp Suite para interceptar as requisições realizadas pelo navegador.

### 2. Interceptação da requisição

A requisição de login foi interceptada utilizando o Burp Suite.

Durante a análise, foi identificado um cookie contendo informações relacionadas à sessão e ao nível de privilégio do usuário.

Entre essas informações estava o parâmetro:

    Admin=false

Por questões de segurança, os valores dos tokens de sessão não são publicados neste repositório.

### 3. Alteração do nível de privilégio

A requisição foi modificada antes de ser encaminhada ao servidor.

Valor original:

    Admin=false

Valor utilizado no teste:

    Admin=true

Também foi alterado o caminho da requisição para:

    /admin

### 4. Acesso ao painel administrativo

Após o envio da requisição modificada, a aplicação permitiu o acesso ao painel administrativo.

Esse comportamento demonstrou que o mecanismo de autorização estava confiando em uma informação que podia ser modificada pelo cliente.

### 5. Identificação da funcionalidade de exclusão

Dentro do painel administrativo, foi identificada a opção de excluir usuários.

A requisição observada para excluir o usuário `carlos` utilizava o seguinte endpoint:

    /admin/delete?username=carlos

### 6. Interceptação da requisição de exclusão

A solicitação de exclusão foi novamente interceptada utilizando o Burp Suite.

A requisição continha o parâmetro relacionado ao privilégio administrativo.

O valor foi alterado de:

    Admin=false

para:

    Admin=true

### 7. Resultado

Após o envio da requisição modificada, a aplicação permitiu a execução da ação administrativa.

O usuário `carlos` foi excluído e o laboratório apresentou a mensagem indicando que o desafio havia sido solucionado.

## Análise da vulnerabilidade

O problema observado está relacionado à confiança em dados controlados pelo cliente para determinar privilégios.

Um usuário pode modificar informações presentes em uma requisição antes que ela seja enviada ao servidor.

Nesse laboratório, a alteração do valor:

    Admin=false

para:

    Admin=true

foi suficiente para obter acesso a funcionalidades administrativas.

Isso demonstra a importância de realizar a verificação de autorização no lado do servidor, em vez de confiar em um valor fornecido pelo cliente.

## Ferramentas utilizadas

### Burp Suite

Utilizado para:

- Interceptar requisições HTTP;
- Visualizar cookies e parâmetros;
- Modificar requisições antes do envio;
- Analisar o comportamento da aplicação após as alterações.

### Firefox

Utilizado para acessar o laboratório e realizar as interações com a aplicação web.

### PortSwigger Web Security Academy

Ambiente utilizado para realizar o laboratório em um cenário controlado.

## Evidências

### 1. Login e interceptação

![Login e interceptação](evidencias/01-login-interceptacao.png)

Primeiro, foi realizado o login com o usuário fornecido pelo laboratório e a requisição foi interceptada pelo Burp Suite.

### 2. Acesso ao painel administrativo

![Acesso ao painel administrativo](evidencias/02-acesso-admin.png)

Após a alteração do parâmetro de privilégio, foi possível acessar a área administrativa.

### 3. Exclusão do usuário Carlos

![Exclusão do usuário Carlos](evidencias/03-exclusao-carlos.png)

A requisição de exclusão foi interceptada e analisada no Burp Suite.

### 4. Laboratório solucionado

![Laboratório solucionado](evidencias/04-lab-resolvido.png)

Após a execução da ação, o laboratório apresentou a mensagem de conclusão e o usuário `carlos` foi removido.

## O que aprendi

Com este laboratório, pratiquei:

- Interceptação de requisições HTTP;
- Análise de cookies;
- Análise de parâmetros enviados pelo cliente;
- Manipulação de requisições utilizando Burp Suite;
- Identificação de mecanismos de controle de acesso;
- Análise de autorização;
- Identificação de informações controladas pelo cliente;
- Análise de endpoints administrativos;
- Documentação de uma vulnerabilidade em ambiente controlado.

## Conclusão

O laboratório demonstrou, de forma prática, como uma aplicação pode apresentar uma falha de controle de acesso quando utiliza informações controladas pelo cliente para determinar privilégios.

Através do Burp Suite, foi possível observar a requisição, identificar o parâmetro `Admin` e testar sua alteração.

A alteração do valor permitiu acessar a área administrativa e executar uma ação que deveria estar restrita a usuários autorizados.

A principal lição do laboratório foi compreender que informações relacionadas à autorização devem ser devidamente validadas pelo servidor e não devem depender exclusivamente de valores que o cliente pode modificar.

## Ambiente de teste

Esta atividade foi realizada exclusivamente no laboratório disponibilizado pela PortSwigger Web Security Academy.

Nenhum sistema de terceiros foi utilizado durante a prática.


