# SDW2023
Santander Dev Week 2023 (ETL com Python)

## Contexto 
Você é um cientista de dados no Santander e recebeu a tarefa de envolver seus clientes de maneira mais personalizada. Seu objetivo é usar o poder da IA Generativa para criar mensagens de marketing personalizadas que serão entregues a cada cliente.



|    UserID      |
|----------------|
|1               |
|2               |
|3               |

1. Seu trabalho é consumir o endpoint GET https://sdw-2023-prd.up.railway.app/users/{id} (API da Santander Dev Week 2023) para obter os dados de cada cliente.
2. Depois de obter os dados dos clientes, você vai usar a API do ChatGPT (OpenAI) para gerar uma mensagem de marketing personalizada para cada cliente. Essa mensagem deve enfatizar a importância dos investimentos.

3. Uma vez que a mensagem para cada cliente esteja pronta, você vai enviar essas informações de volta para a API, atualizando a lista de "news" de cada usuário usando o endpoint PUT https://sdw-2023-prd.up.railway.app/users/{id}.

>`# Utilize sua própria URL se quiser ;)`
> [Repositório da API:](https://github.com/digitalinnovationone/santander-dev-week-2023-api)
>`sdw2023_api_url = 'https://sdw-2023-prd.up.railway.app'`


## ETL  
Extraia a lista de IDs de usuário a partir do arquivo CSV. Para cada ID, faça uma requisição GET para obter os dados do usuário correspondente.


1. 

>`import pandas as pd`
>`df = pd.read_csv('SDW2023.csv')` 
>`user_ids = df['UserID'].tolist()`
>`print(user_ids)`


2. 

>``import requests``
>``import json``

>``def get_user(id):``
> `` response = requests.get(f'{sdw2023_api_url}/users/{id}')``
>  ``return response.json() if response.status_code == 200 else None``
>
>``users = [user for id in user_ids if (user := get_user(id)) is not None]``
>``print(json.dumps(users, indent=2))``


3. 
[
    "id": 4,
    "name": "Pyterson",
    "account": {
      "id": 7,
  {
      "number": "00001-1",
      "agency": "0001",
      "balance": 0.0,
      "limit": 500.0
    },
    "card": {
      "id": 4,
      "number": "**** **** **** 1111",
      "limit": 1000.0
    },
    "features": [],
    "news": [
      {
        "id": 9,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Pyterson, invista hoje para garantir um futuro seguro e pr\u00f3spero. Seu futuro agradece!"
      }
    ]
  },
  {
    "id": 5,
    "name": "Pip",
    "account": {
      "id": 8,
      "number": "00002-2",
      "agency": "0001",
      "balance": 0.0,
      "limit": 500.0
    },
    "card": {
      "id": 5,
      "number": "**** **** **** 2222",
      "limit": 1000.0
    },
    "features": [],
    "news": [
      {
        "id": 10,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Invista hoje para um futuro seguro e est\u00e1vel, Pip. O seu futuro financeiro depende disso!"
      }
    ]
  },
  {
    "id": 6,
    "name": "Pep",
    "account": {
      "id": 9,
      "number": "00003-3",
      "agency": "0001",
      "balance": 0.0,
      "limit": 500.0
    },
    "card": {
      "id": 6,
      "number": "**** **** **** 3333",
      "limit": 1000.0
    },
    "features": [],
    "news": [
      {
        "id": 11,
        "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
        "description": "Oi Pep, investir \u00e9 a chave para multiplicar seu dinheiro. N\u00e3o deixe sua grana parada!"
      }
    ]
  }
]``