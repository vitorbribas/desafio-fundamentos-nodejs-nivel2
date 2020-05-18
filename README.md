# Descrição
Esta aplicação implementa um pequeno sistema de lançamento de transações e cálculo de balanço total.

Uma transação apresenta título, valor e tipo (entrada ou saída).

# Funcionalidades
- Listagem de transações uma a uma e balanço/saldo atual
- Lançamento de uma transação
- Cálculo de balanço
- Validação de saldo suficiente para lançamento de nova saída

# Conceitos exercitados
- Separação da aplicação em rotas, serviços, repositórios e modelos (*models*). [Ler mais sobre](https://www.notion.so/Repository-service-e-patterns-82419cceb11c4c4fbbc055ade7fb1ac5).
- Aplicação de conceitos importantes da Engenharia de Software como: 
    - **SRP** (*Single Responsiblity Principle*)
    - **DPI** (*Dependency Invertion Principle*)
    - **SoC** (*Separation of Concerns*)
    - **DRY** (*Don't Repeat Yourself*)
- Manipulação simples de datas utilizando a biblioteca [date-fns](https://date-fns.org/)
- Tratamento de erros e excessões no Node.js

# Setup
Após clonar o projeto em sua máquina e já ter um ambiente devidamente preparado para o Node.js,
execute o seguinte comando na raiz do projeto para instalar as dependências necessárias:
```
yarn
```
Inicialize a aplicação:
```
yarn dev:server
```
# Interagindo com a aplicação
Utilize alguma ferramenta para requisições HTTP (Insomnia, Postman, entre outros) para interagir com a API.

## Lançar uma transação
Crie algumas transações do tipo entrada ou saída, como as abaixo:
```javascript
// http://localhost:3333/transactions
// método POST

{
	"title": "Salário",
	"value": 1000,
	"type": "income"
}
```
```javascript
// http://localhost:3333/transactions
// método POST

{
	"title": "Cartão de crédito",
	"value": 550,
	"type": "outcome"
}
```
## Ver todas as transações e balanço
Veja todas as transações e balanço total:
```javascript
// http://localhost:3333/transactions
// método GET

{
  "transactions": [
    {
      "id": "dc90dda9-c237-48cb-bb17-35083e2e8848",
      "title": "Salário",
      "value": 1000,
      "type": "income"
    },
    {
      "id": "d91acbba-55f7-4ea9-af76-1be9378a3153",
      "title": "Cartão de crédito",
      "value": 550,
      "type": "outcome"
    }
  ],
  "balance": {
    "income": 1000,
    "outcome": 550,
    "total": 450
  }
}
```
## Simular transação inválida
Uma transação pertence apenas ao tipo **entrada** (*income*) ou **saída** (*outcome*), caso contrário não é possível lançá-la:
```javascript
// http://localhost:3333/transactions
// método POST

{
	"title": "Cartão de crédito",
	"value": 550,
	"type": "out"
}

// Resposta
// Status: 400 (Bad Request)
// Retorno:

{
  "error": "Tipo de transação inválida."
}
```

## Simular saldo insuficiente para lançamento de saída:
```javascript
// http://localhost:3333/transactions
// método POST

{
	"title": "Viagem",
	"value": 600,
	"type": "outcome"
}

// Resposta
// Status: 400 (Bad Request)
// Retorno:

{
  "error": "Seu caixa está abaixo do necessário para esta transação."
}
```
> Considerando as transações exemplo já criadas [acima](#lançar-uma-transação).

# Testando
Rode os testes disponibilizados pelo template:
```
yarn test
```
