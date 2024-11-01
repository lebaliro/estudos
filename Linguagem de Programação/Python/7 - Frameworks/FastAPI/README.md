# 1 Instalação

# 2 Recursos
## 2.1 Types Hints
O Python possui suporte para "dicas de tipo" ou "type hints" (também chamado de "anotações de tipo" ou "type annotations"). Esses "type hints" são uma sintaxe especial que permite declarar o tipo de uma variável.

O FastAPI é baseado nesses type hints, eles oferecem muitas vantagens e benefícios.

Com o FastAPI, você declara parâmetros com type hints e obtém:
- Suporte ao editor.
- Verificações de tipo.

... e o FastAPI usa as mesmas declarações para:

- Definir requisitos: dos parâmetros de rota, parâmetros da consulta, cabeçalhos, corpos, dependências, etc.
- Converter dados: da solicitação para o tipo necessário.
- Validar dados: provenientes de cada solicitação:
- Gerando erros automáticos retornados ao cliente quando os dados são inválidos.

Documenta a API usando OpenAPI:
- que é usado pelas interfaces de usuário da documentação interativa automática.

### 2.1.1 Benefícios
**Interface**

Adiciona uma interface que facilita o programador encontrar os métodos específicos referente aquela variável, objeto, parâmetro, etc.

Sem o type hint:
![image](https://github.com/user-attachments/assets/0d11ae68-f8d1-466c-b13d-afedece726ec)

Com o type hint:
![image](https://github.com/user-attachments/assets/b4bea5b0-18ab-4739-89f3-b0d0d83dc8b9)

**Antecipação de Erro**

![image](https://github.com/user-attachments/assets/c566fbe0-3953-4c9f-9b0b-9eab01054eae)

### 2.1.2 Declarando Tipos
**Tipos simples**

Você pode declarar todos os tipos padrão de Python, não apenas str.

Você pode usar, por exemplo:
- int
- float
- bool
- bytes

```python
def get_items(item_a: str, item_b: int, item_c: float, item_d: bool, item_e: bytes):
    return item_a, item_b, item_c, item_d, item_d, item_e
```

**Tipos genéricos**

Você pode usar, por exemplo:
- List
- Tuple
- Set
- Dict
- Union
- Optional
- ...entro outros.

```python
from typing import List, Set, Tuple, Dict, Union, Optional

def process_items(
    items: List[str],
    items_t: Tuple[int, int, str],
    items_s: Set[bytes],
    prices: Dict[str, float],

    item: Union[int, str],
    name: Union[str, None] (é o mesmo que) :Optional[str] (que é o mesmo que) :str | None
):
```

**Classes como Tipos**
```python
class Person:
    def __init__(self, name: str):
        self.name = name


def get_person_name(one_person: Person):
    return one_person.name
```

![image](https://github.com/user-attachments/assets/2f297b76-4881-4781-b162-d402ad6c52b9)

### 2.1.3 Type Hints com Metadados de Anotações
O Python possui uma funcionalidade que nos permite incluir metadados adicionais nos type hints utilizando Annotated.

from typing import Annotated


def say_hello(name: Annotated[str, "this is just metadata"]) -> str:
    return f"Hello {name}"

O Python em si não faz nada com este Annotated. E para editores e outras ferramentas, o tipo ainda é str.

Mas você pode utilizar este espaço dentro do Annotated para fornecer ao FastAPI metadata adicional sobre como você deseja que a sua aplicação se comporte.

## 2.2 Pydantic
O Pydantic é uma biblioteca Python para executar a validação de dados. O FastAPI é todo baseado em Pydantic.

```python
from datetime import datetime

from pydantic import BaseModel


class User(BaseModel):
    id: int
    name: str = "John Doe"
    signup_ts: datetime | None = None
    friends: list[int] = []


external_data = {
    "id": "123",
    "signup_ts": "2017-06-01 12:22",
    "friends": [1, "2", b"3"],
}
user = User(**external_data)
print(user)
# > User id=123 name='John Doe' signup_ts=datetime.datetime(2017, 6, 1, 12, 22) friends=[1, 2, 3]
print(user.id)
# > 123
```
