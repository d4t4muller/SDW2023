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


1. Iremos importar a biblioteca pandas e definir o dataframa como o csv feito anteriormente, declarando cada coluna na tabela para cada ID.

>  Importando bibliotecas necessarias
```
import pandas as pd
import requests
import json
```


> declarando cada coluna na tabela para cada ID
```
df = pd.read_csv('SDW2023.csv')
User_ids = df['UserID'].tolist()
print(user_ids)

```


2.  Codando a função para retorno dos valores.


> Declaramos a função get_user() onde response armazera o id e retorna um json caso a resposta for 200

```
def get_user(id):
    response = requests.get(f'{sdw2023_api_url}/users/{id}')
    return response.json() if response.status_code == 200 else None

users = [user for id in user_ids if (user := get_user(id)) is not None]
print(json.dumps(users, indent=2))

```


3.  Isto ira te retornar um arquivo.json com mais ou menos a sintaxe a seguir:
```
[   {
        "id": 404,
        "name": "d4t4muller",
        "account": 
        "id": 7
    },
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
    "features": []
]
```