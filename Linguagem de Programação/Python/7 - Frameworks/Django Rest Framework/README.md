# Introdução
Essa documentação tem como objetivo resumir informações para pesquisas rápidas, caso seja necessário um maior detalhamento, recomenda-se acessar o link no título de cada tema.

# Palavras Chaves
Uma lista de palavras chaves que estão associados diretamente a teoria e prática do framework:
- Modelos
- Autenticação e Autorização
- Serializadores
- Views
- Routers

# [Serializadores](https://www.django-rest-framework.org/api-guide/serializers/)
Os serializadores permitem que dados complexos, como querysets e instâncias de modelos, sejam convertidos em tipos de dados nativos do Python que podem ser facilmente renderizados em JSON, XMLou outros tipos de conteúdo. Os serializadores também fornecem desserialização, permitindo que dados analisados ​​sejam convertidos de volta em tipos complexos, após a primeira validação dos dados recebidos. Os serializadores no framework REST funcionam de forma muito similar as classes Form e ModelForm.

## ModelSerializer
A ModelSerializer fornece um atalho que permite criar automaticamente uma Serializerclasse com campos que correspondem aos campos do Modelo.

O que o diferencia da classe Serializers é:
- Ele gerará automaticamente um conjunto de campos para você, com base no modelo.
- Ele gerará automaticamente validadores para o serializador, como validadores unique_together.
- Inclui implementações padrão simples de .create()e .update().

É possível inspecionar a estrutura de um ModelSerializer utilizando o `print(repr(serializer))`

Pode utilizar o atributo `fields='__all__'` para indicar que todos os campos do modelo serão serializados, como também pode utilizar o atributo `exclude=['users']` para indicar quais campos não devem ser considerados na serialização.

O atributo `depth` é um valor inteiro que indique a profundidade dos relacionamentos que devem ser percorridos, ou seja, pode facilmente gerar representações aninhadas, ao invés de mostrar a FK, mostrará os dados.

Você pode adicionar campos extras ou substituir os campos padrãos, assim como faria para uma classe Serializer.

O atributo `read_only_fields = ['account_name']` torna o campo apenas para leitura.

O atributo `extra_kwargs = {'password': {'write_only': True}}` permite que você especifique argumentos de palavra-chave adicionais arbitrários.

Ficou faltando ler o subtópico Personalizando mapeamentos de campos na documentação oficial, ler depois para enteder melhor.

# Views
## Viewsets
são uma abstração adicional disponível no Django Rest Framework que permite lidar com múltiplas operações (listagem, criação, atualização, exclusão) em um único conjunto de views. Em vez de criar views individuais para cada operação (como listar ou detalhar), você define todas as operações de uma API em uma única classe ViewSet. Em seguida, o roteador do DRF (como o DefaultRouter) configura automaticamente as URLs para cada operação do ViewSet, simplificando ainda mais a criação de APIs RESTful.

É simplesmente um tipo de visualização baseada em classe, que não fornece nenhum manipulador de método como .get()ou .post(), e em vez disso fornece ações como .list()e .create().

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
