# repo_testes

Variaveis de ambiente

Assim como qualquer linguagem de programação, também temos opção de manipular e definir variáveis nos *workflows* do **Github Actions**. 

Dentro deste contexto, temos dois tipos de variáveis:
- Variáveis de ambiente, que podem ser utilizadas no contexto de um único *workflow* (compartilhando informações entre steps).
- Variáveis de configuração, que podem ser definidas a nível de organização, repositório ou ambiente. Elas podem ser compartilhadas no contexto em que foram criadas e compartilhando a informação em todos *workflows* que tem acesso à ela.

Vamos começar falando das variáveis de ambiente.

## Variáveis de ambiente

### Variáveis padrão

Nativamente, o **Github Actions** já disponibiliza algumas variáveis de ambientes padrão. Dentre essas variávieis, existem algumas bem interessante e que podem ser bem úteis no dia-a-dia. São elas:

| Variável | Descrição |  
|----------|-----------|
|  GITHUB_ACTOR        |   Nome do usuário que realizou o commit      |
|  GITHUB_EVENT_NAME        |  Nome do evento que disparou o workflow         |
|  GITHUB_REPOSITORY        |  Nome do repositório em que está rodando o workflow         |
|  GITHUB_SHA        |  O sha do commit que disparou o workflow         |

A lista de todas variáveis disponíveis está [aqui](https://docs.github.com/en/actions/learn-github-actions/variables#default-environment-variables).

Para acessá-las dentro do seu *workflow*, utilize os comandos para acessar as variáveis de ambiente do sistema operacional/linguagem de programação que seu job está rodando.

Exemplos para acessar em linux, windows e através de javascript:
https://github.com/DevCombOps/medium-example/blob/8e94ef5fd90cd2be8a1f1094abd83f7fd9875de5/.github/workflows/example.yml#L5-L33

### Variáveis customizadas

Além das variáveis mecionandas acima, também é possível criar novas variáveis para utilizar em nosso fluxo.

Um dos principais caso de uso quando falamos das variáveis de ambiente customizada, é quando precisamos salvar um determinado valor em um step e usar este mesmo valor em um passo posterior do nosso fluxo.

<!-- precisamos compartilhar uma informação com outros steps dentro de um mesmo workflow, temos duas opções: definir variáveis de output (link pro rafão) ou definir variáveis de ambiente. -->
<!--
Dois casos de uso:
  1 -> preciso definir uma variavel em um step e não vou mais altera-la (output)
  2 -> Preciso definir uma variavel que pode sofrer alteracos em outros steps, neste caso vale env.  -->

Uma das formas de definir uma váriavel dentro do seu workflow, é declarando-a com seu valor inicial no escopo desejado.

No exemplo abaixo, temos:
  - a variável env_var, sendo declarada a nível de workflow, logo, acessível por todos os jobs.
  - a variável job_var, sendo declarada no nível do job display-variables, sendo acessível apenas pelos steps deste job;
  - a variável step_var, sendo declarada no nível do step step-variable, logo, acessível apenas por este step

https://github.com/DevCombOps/medium-example/blob/c13d03ecd679f978b0be7a96d23dec68395e8fa7/.github/workflows/exemplo-escopo-variaveis.yml#L1-L37

Como resultado do código acima, temos o seguinte resultado:

![img_003](https://github.com/guilhermercandido/repo_testes/assets/26474513/668c547a-ddc2-46ea-940b-f36d143d3740)

O GitHub Actions permite uma fácil visualização das variáveis de ambiente customizadas: basta expandir a primeira linha do nosso step que teremos uma propriedade chamada **env**. Ela conterá todas as variáveis que podem ser acessadas por aquele step e o seu valor, facilitando eventuais troubleshooting.

Além das variáveis definidas em cada escopo, também é possível declarar variáveis dentro da seção ***run*** de algum determinado step.
Uma vez iniciado o workflow, o Github utiliza um arquivo de sistema para gerenciamento das variáveis de ambiente disponíveis para cada job. Este arquivo é chamdo ***GITHUB_ENV***.
<!-- Para isso, precisamos definir a variável de ambiente e escrevê-la no arquivo GITHUB_ENV. Este arquivo é utilizado pelo fluxo para definição de todas as variávies de ambiente customizadas. -->

Exemplo para exportar variavel rodando em linux:

https://github.com/DevCombOps/medium-example/blob/b10ad159713af869f00b64a815c291ac0cdbe649/.github/workflows/examplo-variaveis.yml#L35-L53

Execução do código acima:

![img_001](https://github.com/guilhermercandido/repo_testes/assets/26474513/cfa43fa6-612d-442b-918b-f2fbc0fc6c31).

Repare que, no segundo step, temos destacado uma seção chamada **env**. Ela nos indicará quais variáveis de ambiente customizadas estarão disponíveis para o step consultar. 

Exemplo para exportar variavel rodando algum código javascript:

https://github.com/DevCombOps/medium-example/blob/b10ad159713af869f00b64a815c291ac0cdbe649/.github/workflows/examplo-variaveis.yml#L54-L64

Execução do código acima:
![image](https://github.com/DevCombOps/medium-example/assets/26474513/206df647-c677-4154-9269-45bc8545044b)

