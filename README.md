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


[Repositório da API:](https://github.com/digitalinnovationone/santander-dev-week-2023-api)
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
df = pd.read_csv('user.csv')
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
[
  {
    "id": 1015,
    "name": "d4t4muller",
    "account": {
      "id": 1087,
      "number": "99999",
      "agency": "8888",
      "balance": 0.0,
      "limit": 8000.0
    },
    "card": {
      "id": 998,
      "number": "8528 **** **** ***8 ",
      "limit": 10000.0
    },
    "features": [
      {
        "id": 366,
        "icon": "none",
        "description": "none"
      }
    ],
    "news": [
      {
        "id": 2342,
        "icon": "none",
        "description": "none"
      }
    ]
  }
]
```




## **T**ransform 

Utilize a API do OpenAI GPT-4 para gerar uma mensagem de marketing personalizada para cada usuário.

```!pip install openai```



-[Documentação Oficial da API OpenAI:](https://platform.openai.com/docs/api-reference/introduction)
-[Informações sobre o Período Gratuito:](https://help.openai.com/en/articles/4936830)

- Para gerar uma API Key:
-1. Crie uma conta na OpenAI
-2. Acesse a seção "API Keys"
-3. Clique em "Create API Key"
-[Link direto:](https://platform.openai.com/account/api-keys)

>Substitua por sua API Key da OpenAI, ela será salva como uma variável de ambiente.


```openai_api_key = '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'```


## Função para gerar mensagens : 

``` 
import openai

openai.api_key = openai_api_key


def generate_ai_news(user):
  completion = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=[
      {
          "role": "system",
          "content": "Você é um especialista em markting bancário."
      },
      {
          "role": "user",
          "content": f"Crie uma mensagem para {user['name']} sobre a importância dos investimentos (máximo de 100 caracteres)"
      }
    ]
  )
  return completion.choices[0].message.content.strip('\"')

for user in users:
  news = generate_ai_news(user)
  print(news)
  user['news'].append({
      "icon": "https://digitalinnovationone.github.io/santander-dev-week-2023-api/icons/credit.svg",
      "description": news
  })
```




## Load


``` 

def update_user(user):
  response = requests.put(f"{sdw2023_api_url}/users/{user['id']}", json=user)
  return True if response.status_code == 200 else False

for user in users:
  success = update_user(user)
  print(f"User {user['name']} updated? {success}!")```

