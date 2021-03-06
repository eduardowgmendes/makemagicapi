# Make Magic API
  
## O Projeto 
Teste para uma entrevista de emprego de uma empresa de tecnologia. A tarefa era criar uma API REST com operações de um CRUD para inserir, recuperar, atualizar e deletar personagens do universo de *Hogwarts*. A API deveria se integrar a outra API denominada **`Potter API`** para verificar durante as operações de `POST` e `PUT` se a casa inserida correspondia as casas de *Hogwarts* presentes na `Potter API`. Confesso que tive que assistir aos primeiros filmes para me inteirar sobre a história e entender de fato como tudo funcionaria. 

Inteiramente concebido na linguagem Java utilizando o Spring e outras dependências como Spring Data com MySQL, Spring Cache utilizando o `Redis`. Se quiser conhecer a API em mais detalhes vá para a seção [Visão Geral.](https://github.com/eduardowgmendes/makemagicapi#vis%C3%A3o-geral) *Ao final haverá uma lista resumida de todas as tecnologias utilizadas nesse projeto.*      


## Build

Você pode testar a API de duas formas:

* Através do Heroku utilizando o Postman para realizar as requisições

* Local via Docker seguindo as instruções da seção [Docker](https://github.com/eduardowgmendes/makemagicapi#docker) neste documento.     

Na primeira execução local o banco de dados estará vazio, portanto a primeira requisição ao endpoint `/api/v1/personas` retornará `204 - No Content`. Utilize o mesmo endpoint mas efetuando um `POST` para inserir qualquer personagem seguindo as orientações da seção [Operações do CRUD - Criar Personagem.](https://github.com/eduardowgmendes/makemagicapi#criar-um-personagem) 


### Heroku 

Apenas para testes o deploy foi realizado no Heroku que é uma plataforma em nuvem como um serviço *PaaS* que suporta várias linguagens de programação. *Nota Devido à notável lentidão da plataforma é recomendável acessar os endpoints abaixo utilizando do Postman.*   

| Método  |  Link  |
| ------------------- | ------------------- |
|  `GET`  |  `https://makemagic-api.herokuapp.com/api/v1/personas` |
|  `GET`  |  `https://makemagic-api.herokuapp.com/api/v1/personas{id}` |
|  `POST`  |  `https://makemagic-api.herokuapp.com/api/v1/personas` |
|  `PUT`  |  `https://makemagic-api.herokuapp.com/api/v1/personas/{id}` |
|  `DELETE`  |  `https://makemagic-api.herokuapp.com/api/v1/personas/{id}` |


#### Todos os Personagens 
[https://makemagic-api.herokuapp.com/api/v1/personas](https://makemagic-api.herokuapp.com/api/v1/personas)

#### Personagem por `id` 
[https://makemagic-api.herokuapp.com/api/v1/personas/1](https://makemagic-api.herokuapp.com/api/v1/personas)

#### Documentação Swagger 
[https://makemagic-api.herokuapp.com/swagger-ui.html](https://makemagic-api.herokuapp.com/swagger-ui.html)

*Nota: O `ClearDB` do Heroku, em seu plano gratuito, incrementa os ids das entidades de 10 em 10 portanto ao testar via Heroku você notará esse comportamento, como explicado nesse link: [https://w2.cleardb.net/faqs/#general_16](https://w2.cleardb.net/faqs/#general_16)*

## Docker
Abra o terminal na raiz do projeto no diretório `makemagicapi` e digite o comando: `sudo docker up --build`. 

*Nota: Se houver algum problema com o MySQL na hora de subir a aplicação Spring, apenas pare a operação do Docker utilizando o `CTRL+C` duas vezes e reinicie novamente com o comando: `sudo docker up --build`.*        


## Visão Geral

Aqui serão explicados informações importante acerca da API, quais foram as decisões de projeto e implementação assim como os detalhes dos requisitos solicitados e as tecnologias utilizadas. *Vá para a seção [Build](https://github.com/eduardowgmendes/makemagicapi#build) se quiser pular essa parte e testar a API.*  

### `Persona` != `Character`
Decidi ao implementar a solução, optar por um nome de entidade diferente da recomendada pelo teste. Isso se deu ao fato de eu me familiarizar melhor com o nome **Persona** do que **Character** visto que no Java `java.lang.Character` é uma das classes Wrapper que compõem a linguagem em si. *Isso não foi um problema no geral, mas resolvi utilizar outro nome para facilitar.*           

### Casas
Como requisito principal do teste, todas as propriedades pertencentes aos personagens podem ser atualizadas com exceção de apenas o `id` da `house` que representa uma das quatro casas de *Hogwarts* listadas abaixo:     

| Casa       | Id da Casa               |
|------------|--------------------------|
| `Slytherin`  | `5a05dc8cd45bd0a11bd5e071` |
| `Gryffindor` | `5a05e2b252f721a3cf2ea33f` |
| `Hufflepuff` | `5a05dc58d45bd0a11bd5e070` |
| `Ravenclaw`  | `5a05da69d45bd0a11bd5e06f` |  

**Se você tentar atualizar a propriedade `house` de um personagem com um valor diferente receberá uma mensagem de erro.**  


### Cache 
Para este projeto foi utilizado o **Redis** para armazenar o cache durante as operações de `GET` que são mais custosas ao banco de dados como por exemplo, a listagem de todos os personagens.    

## Endpoints 
Os principais `endpoints` da Api:  

| Método  |  Endpoint  |
| ------------------- | ------------------- |
|  `GET`  |  `/api/v1/personas` |
|  `GET`  |  `/api/v1/personas{id}` |
|  `POST`  |  `/api/v1/personas` |
|  `PUT`  |  `/api/v1/personas/{id}` |
|  `DELETE`  |  `/api/v1/personas/{id}` |


## Filtros
É possível filtrar todos os personagens pelas suas respectivas casas através do `endpoint`:  
* `GET` - `/api/v1/personas?house={houseId}`


## Operações do CRUD 

### Todos os Personagens  
#### GET - `/api/v1/personas`
Para recuperar a lista completa de personagens utilize o endpoint `/api/v1/personas`. Se desejar filtrar os personagens pelas suas respectivas casas adicione como parâmetro a este endpoint `?house={houseId}` onde `houseId` pode ser uma das quatro casas mencionadas. Retorna `204 - No Content` se a lista estiver vazia ou o parâmetro for vazio como em `?house= `.  


### Personagem por seu `id`  
#### GET - `/api/v1/personas/{id}`
Para recuperar apenas um personagem utilize o endpoint `/api/v1/personas/{id}`, onde `id` é o id respectivo do personagem. 
Retorna `404 - Not Found` se não houver um personagem com o `id` informado. 


### Criar um Personagem  
#### POST - `/api/v1/personas`
Para inserir um novo registro você deve utilizar o endpoint `/api/v1/personas`. É necessário passar no body da requisição os seguintes dados excluindo-se a propriedade id: 

```json
      {
        "name": "Harry Potter",
        "role": "student",
        "school": "Hogwarts School of Witchcraft and Wizardry",
        "house": "5a05dc8cd45bd0a11bd5e071",
        "patronus": "stag"
      }	
```

Retorna `400 - Bad Request` com uma mensagem pertinente se a propriedade `house` for diferente de uma das quatro opções de casas mencionadas. 


### Atualizar um Personagem  
#### PUT - `/api/v1/personas/{id}`
Para atualizar o registro de um personagem específico você deve utilizar o endpoint `/api/v1/personas/{id}`, onde a propriedade `id` é o id específico do personagem ao qual deseja-se atualizar. Pode-se modificar todas propriedades que considerar necessárias porém a propriedade `house` deve ser uma das quatro opções. 

Retorna `400 - Bad Request` com uma mensagem pertinente se a propriedade `house` for diferente de uma das quatro opções de casas mencionadas.   

### Deletar um Personagem  
#### DELETE - `/api/v1/personas/{id}`
Para deletar um personagem específico você deve utilizar o endpoint `/api/v1/personas/{id}`, onde a propriedade `id` é o id específico do personagem ao qual deseja-se deletar. Retorna `404 - Not Found` se não houver um personagem com o `id` informado
 
## Documentação Swagger
Para acessar a documentação do Swagger utilize o endpoint `/swagger-ui.html`.

 
## Resumo das Tecnologias
Abaixo segue uma lista resumida de tecnologias utilizadas na concepção desse projeto: 
* Docker
* Maven 3.6.3 
* Redis
* Swagger2   
* Spring Data JPA MySQL  
* Retrofit 2.3.0    
* Lombok 
