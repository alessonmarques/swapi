# swapi

## Implementação da API para avaliação de habilidades.

- Foi construída uma aplicação FrontEnd utilizando o framework Vue.JS para o consumo e visualização dos dados da API: https://swapi.dev/;
- Os métodos implementados foram: 
```javascript
loadInititalData() //Responsável por carregar os dados iniciais.
loadPeoplePageData() //Responsável por carregar os dados das interações com as páginas.
loadPageResults() //Responsável por tratar os dados e salva-los para as funções anteriores.
loadPeopleData() //Responsável por carregar os dados após selecionada um elemento da lista 'People'.
searchPeoples(): //Responsável por fazer a busca utilizando a query search para habilitar a buscar os elementos pelo nome.
```
- Para as consultas à api foi utilizado a biblioteca Axios.

## Inicialização do projeto.
- O projeto pode ser iniciado utilizando docker, para isso após ter o projeto em sua máquina só precisa rodar o seguinte comando: (dentro da pasta do projeto)
```shell
docker build -t swapi:dev .
docker run -d -v ${PWD}:/app -v /app/node_modules -p 8081:8080 --rm --name swapi-container swapi:dev
```

