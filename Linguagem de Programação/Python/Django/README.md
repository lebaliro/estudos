# Recursos
Essa parte será dedicada a anotar pontos importantes para consultas posteriores, para um melhor detalhamento é necessário acessar a documentação oficial.

## Models
### [Lista de Informações Úteis](https://docs.djangoproject.com/pt-br/5.1/topics/db/models/)
- Os atributos da classe model representam os campos no banco de dados. Um campo id é adicionado automaticamente, mas este comportamento pode ser sobrescrito;
- Para indicar ao Django que você irá utilizar um modelo é necessário instalar o módulo que possui esse modelo dentro da constante INSTALLED_APPS, que fica no settings.py do módulo principal do Django;
- Os campos são a parte mais importante e é a única parte requerida de um modelo;
- O Django usa o tipo de classe do campo para determinar algumas coisas:
  - O tipo da coluna, o qual diz ao banco de dados que tipo de dado irá armazenar (eg INTEGER, VARCHAR, TEXT);
  - O widget HTML padrão para usar quando renderizar o campo de um form (ex. <input type="text":>, <select:>);
  - Os requisitos mínimos de validação, usados ​​no administrador do Django e em formulários gerados automaticamente.
- Você pode facilmente escrever seus próprios campos se os que vêm definidos no Django não resolverem;
- Cada tipo de campo, exceto para ForeignKey, ManyToManyField e OneToOneField, recebe um argumento opcional como primeiro argumento – um nome detalhado;
- O Django oferece maneiras de definir os três tipos de relacionamentos mais comuns: muitos-para-muitos, muitos-para-um e um-para-um;
- Django impõe algumas restrições aos nomes dos campos do modelo:
  - Um nome de campo não pode ser uma palavra reservada do Python;
  - Um nome de campo não pode conter mais de um sublinhado em uma linha;
  - Um nome de campo não pode terminar com um sublinhado, por razões semelhantes.
- Forneça metadados, opcionalmente, ao seu modelo através da classe Meta;
- O atributo mais importante de um modelo é o Manager. É a interface por meio da qual as operações de consulta de banco de dados são fornecidas aos modelos Django e é usada para recuperar as instâncias do banco de dados. Se nenhum personalizado Managerfor definido, o nome padrão é objects. Os gerenciadores são acessíveis somente por meio de classes de modelo, não das instâncias de modelo.
- Defina métodos personalizados em um modelo para adicionar funcionalidade personalizada de “nível de linha” aos seus objetos. Enquanto os métodos Managers são destinados a fazer coisas “em toda a tabela”, os métodos de modelo devem agir em uma instância de modelo específica.
- Existem alguns métodos automaticamente fornecidos pelos modelos que você talvez gostará de substituir: __str__(), get_absolute_url(), save(), delete();
- Métodos de modelo substituídos não são chamados em operações em massa
- Há três estilos de herança possíveis no Django:
  - Classes Base Abstrata
  - Herança de múltiplas tabelas
  - Modelos Proxy
### [Campos](https://docs.djangoproject.com/en/5.1/ref/models/fields/)
#### Tipos de Campos

#### Opções de Campos
Os seguintes argumentos estão disponíveis para todos os tipos de campo. Todos são opcionais.

- null se refere ao banco de dados, enquanto blank se refere a validação do formulário.
- choices - para fazer escolhas pre-definidas e validáveis
- default - O valor padrão para o campo
- help_text - Texto extra de “ajuda” a ser exibido com o widget de formulário. É útil para documentação mesmo se seu campo não for usado em um formulário.
- error_messages - permite que você substitua as mensagens padrão que o campo irá levantar.
- primary_key - este campo é a chave primária do modelo
- unique - este campo deve ser único em toda a tabela
- unique_for_date / unique_for_month / unique_for_year - Defina isso como o nome de um DateFieldou DateTimeFieldpara exigir que este campo seja exclusivo para o valor do campo de data, mês ou ano
- verbose_name - Um nome legível para o campo.
- validators - Uma lista de validadores para executar para este campo. 
- db_default - Será definido no nível do banco de dados e será usado ao inserir linhas fora do ORM ou ao adicionar um novo campo em uma migração.
- db_column - O nome da coluna do banco de dados a ser usada para este campo.
- db_comment - O comentário na coluna do banco de dados a ser usada para este campo
- db_index - um índice de banco de dados será criado para este campo
- db_tablespace - O nome do tablespace do banco de dados a ser usado para o índice deste campo, se este campo for indexado.
- editable - Se False, o campo não será exibido no admin ou em qualquer outro ModelForm. Eles também são ignorados durante a validação do modelo.
