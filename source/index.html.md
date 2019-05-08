---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - JSON

toc_footers:
  - <a href='mailto:tech-yoda@conductor.com.br'>Suporte</a>

includes:
  - errors

search: true
---

# Introdução

Bem vindo a documentação da API de conciliação de boletos. Este serviço disponibiliza algumas consultas que permitem obter diversas informações relacionadas a emissão e liquidação de boletos.

As informações disponíveis na API, são geradas por outros serviços:

### Projetos relacionados
Projeto | URL
--------- | -----------
yoda-billet-s3-event | https://github.com/cdt-baas/yoda-billet-s3-event
yoda-billet-data | https://github.com/cdt-baas/yoda-billet-data
yoda-billet-extractor | https://github.com/cdt-baas/yoda-billet-extractor

<aside class="notice">
Você precisa de credenciais válidas para consultar este serviço em qualquer ambiente como DEV, HML ou PRD.
</aside>

# Boletos

## Listar Boletos
`/yoda-billet-api/billets`
> JSON de retorno:

```json
[
  'pagination': {
    "total": Integer,
    "total_pages": Integer,
    "current_page": Integer,
    "page_size": Integer  
  }
  'billets' : [
    {
      "id_billet_full": Integer,
      "document_number": String,
      "amount": Float,
      "received_date": Date,
      "id_account": Integer,
      "id_issuer": Integer,
      "id_billet_pier" : Integer,
      "status": Integer,
      "processing_date": Date,
      "due_date": Date,
      "payment_date": Date,
      "reconciliation_status": Integer 
    },
    {
      "id_billet_full": Integer,
      "document_number": String,
      "amount": Float,
      "received_date": Date,
      "id_account": Integer,
      "id_issuer": Integer,
      "id_billet_pier" : Integer,
      "status": Integer,
      "processing_date": Date,
      "due_date": Date,
      "payment_date": Date,
      "reconciliation_status": Integer 
    }
  ]
]
```
### Parametros de consulta/filtro

Parametro | Obrigatório | Descrição
--------- | ------- | -----------
status | falso | Status do boleto (1 - Emitido, 2 - Cancelado s/ registro, 3 - Registrado, 4 - Pago, 5 - Cancelado c/ registro) 
reconciliation_status | false | Filtrar por status da conciliação (1 - Conciliado, 0 - Não conciliado)
payment_date | false | Caso o boleto esteja pago, a consulta poderá ser feita por data


## Resumo da conciliação diária
`/yoda-billet-api/summary`
> JSON de retorno:

```json
[
  {
    "issuer": String,
    "total_items": Integer,
    "total_amount" Float
  },
  {
    "issuer": String,
    "total_items": Integer,
    "total_amount" Float
  }
]
```
### Parametros de consulta/filtro

Parametro | Obrigatório | Descrição
--------- | ------- | -----------
payment_date | Verdadeiro | Data para consultar as conciliações do dia


## Paginação
> Estrutura gerada dentro do retorno:

```json
  [
    'pagination': {
      "total": Integer,
      "total_pages": Integer,
      "current_page": Integer,
      "page_size": Integer  
    }
  ]
```
Os recursos que retornam mais que 50 registros possuem um paginador gerado pelo SQLAlchemy + Flask, os seguinte parametros devem ser informados na query:

Parametro | Obrigatório | Descrição
--------- | ------- | -----------
page | Verdadeiro | Página atual
page_size | Verdadeiro | Tamanho da página
