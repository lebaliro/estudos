# Introdução
Essa documentação tem como objetivo resumir informações para pesquisas rápidas, caso seja necessário um maior detalhamento, recomenda-se acessar o link no título de cada tema.

# Palavras Chaves
Uma lista de palavras chaves que estão associados diretamente a teoria e prática do framework:
- Modelos
- Autenticação e Autorização
- Serializadores
- Views, Mixins/Generics e ViewSets
- Routers

# [Testes](https://www.django-rest-framework.org/api-guide/testing/#explicitly-encoding-the-request-body)
## APIRequestFactory
O código abaixo mostra como pode ser feito uma requisição de teste:

```python
from rest_framework.test import APIRequestFactory

# Using the standard RequestFactory API to create a form POST request

factory = APIRequestFactory()
request = factory.post('/notes/', json.dumps({'title': 'new idea'}), format='json', content_type='application/json')
```
Ela costuma retornar a requisição e não a resposta.

- Métodos que criam um corpo de solicitação, como post, put e patch, incluem um argumento `format`, o que facilita a geração de solicitações usando um tipo de conteúdo diferente de dados de formulário multipartes.
- Se você precisar codificar explicitamente o corpo da solicitação, você pode fazer isso definindo o sinalizador `content_type`.

## Autenticação forçada
Ao testar visualizações diretamente usando uma fábrica de solicitações, geralmente é conveniente poder autenticar diretamente a solicitação, em vez de ter que construir as credenciais de autenticação corretas. Para autenticar uma solicitação à força, use o método `force_authenticate()`.

A documentação explicita mais detalhes sobre autenticação por tokens.

## APIClient
A classe `ÀPIClient` extende da classe `Client` do Django Padrão. Ela costuma retorna a resposta de uma requisição.

```python
from rest_framework.test import APIClient

client = APIClient()
client.post('/notes/', {'title': 'new idea'}, format='json')
```
O APIClient.credentials() é um método apropriado para testar APIs que exigem cabeçalhos de autenticação, como autenticação básica, autenticação OAuth1a e OAuth2 e esquemas de autenticação de token simples.

## RequestsClient e CoreAPIClient
Com o uso cuidadoso, tanto o RequestsClient como o CoreAPIClient fornecem a capacidade de escrever casos de teste que podem ser executados em desenvolvimento ou diretamente no seu servidor de preparação ou ambiente de produção.

Usar esse estilo para criar testes básicos de algumas peças principais de funcionalidade é uma maneira poderosa de validar seu serviço ativo. Fazer isso pode exigir alguma atenção cuidadosa à configuração e desmontagem para garantir que os testes sejam executados de uma forma que não afetem diretamente os dados do cliente.

## Classes de Teste
A estrutura REST inclui as seguintes classes de caso de teste, que espelham as classes de caso de teste existentes do Django:
- APISimpleTestCase
- APITransactionTestCase
- APITestCase
- APILiveServerTestCase

O atributo `self.client` será uma instância do APIClient.

O `response.data` possui os dados da requisição.
