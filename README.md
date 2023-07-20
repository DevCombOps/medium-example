# repo_testes

Variaveis de ambiente

Assim como qualquer linguagem de programação, também temos opção de manipular e definir variáveis nos *workflows* do **Github Actions**. 

Dentro deste contexto, temos dois tipos de variáveis:
- Variáveis de ambiente, que podem ser utilizadas no contexto de um único *workflow* (compartilhando informações entre steps).
- Variáveis de configuração, que podem ser definidas a nível de organização, repositório ou ambiente. Elas podem ser compartilhadas no contexto em que foram criadas e compartilhando a informação em todos *workflows* que tem acesso à ela.

Vamos começar falando das variáveis de ambiente.

## Variáveis de ambiente

### Variáveis padrão

Nativamente, o **Github Actions** já disponibiliza algumas variáveis de ambientes padrão. Dentre essas variávieis, podemos destacar algumas como:

| Variável | Descrição |  
|----------|-----------|
|  GITHUB_ACTOR        |           |
|  GITHUB_BASE_REF        |           |
|  GITHUB_EVENT_NAME        |           |
|  GITHUB_REF        |           |
|  GITHUB_REF_NAME        |           |
|  GITHUB_EVENT_NAME        |           |
|  GITHUB_EVENT_NAME        |           |
|  GITHUB_EVENT_NAME        |           |
|  GITHUB_EVENT_NAME        |           |

A lista de todas variáveis disponíveis está [aqui](https://docs.github.com/en/actions/learn-github-actions/variables#default-environment-variables).

Para acessá-las dentro do seu *workflow*, utilize os comandos para acessar as variáveis de ambiente do sistema operacional que seu job está rodando.

Exemplo em unix:
https://github.com/guilhermercandido/repo_testes/blob/3fecdcbbf4243a98a60bd108b831847c69d445a0/.github/workflows/1-teste.yml#L6-L16

Exemplo em windows:
https://github.com/guilhermercandido/repo_testes/blob/3fecdcbbf4243a98a60bd108b831847c69d445a0/.github/workflows/1-teste.yml#L6-L16

### Variáveis customizadas

Além das variáveis mecionandas acima, também é possível criar novas variáveis para utilizar em nosso fluxo.

Um dos principais caso de uso quando falamos das variáveis de ambiente customizadas, é quando precisamos salvar um determinado valor em um step, sendo que precisaremos consultar este mesmo valor em um passo posterior do nosso fluxo.

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

https://github.com/guilhermercandido/repo_testes/blob/2f968202da1f0f1f5ebb9f711ef41f157a37f8f3/.github/workflows/2-variables.yml#L1-L28

Como resultado do código acima, temos o seguinte resultado:
![img_003](https://github.com/guilhermercandido/repo_testes/assets/26474513/668c547a-ddc2-46ea-940b-f36d143d3740)

Um ponto interessante é que o GITHUB permite uma fácil visualização das variáveis de ambiente customizadas. Basta expandir a primeira linha do nosso step que teremos uma propriedade chamada **env**. Ela conterá todas as variáveis que podem ser acessadas por aquele step e o seu valor, facilitando o troubleshooting.

Porém, muitas vezes, precisamos que esse valor seja alterado ou definido em tempo de execução do workflow. 
Uma vez iniciado o workflow, o Github utiliza um arquivo de sistema para gerenciamento das variáveis de ambiente disponíveis para cara job. Este arquivo é chamdo GITHUB_ENV.
<!-- Para isso, precisamos definir a variável de ambiente e escrevê-la no arquivo GITHUB_ENV. Este arquivo é utilizado pelo fluxo para definição de todas as variávies de ambiente customizadas. -->

Para exemplificação, vamos fazer um paralelo ao método de definição de variáveis de ambiente para sistemas operacionais unix.

Neste tipo de SO, para exportar uma varíavel de ambiente, utilizamos o comando *export*. Fazendo este mesmo comando em um step no Github Actions, temos o seguinte resultado:

https://github.com/guilhermercandido/repo_testes/blob/16d4961aef2c5307b081963dabd0b7f4070873d5/.github/workflows/1-teste.yml#L6-L19

Execução do código acima:

![img_001](https://github.com/guilhermercandido/repo_testes/assets/26474513/cfa43fa6-612d-442b-918b-f2fbc0fc6c31)

Neste caso, mesmo definindo a variável de ambiente *$teste*, repare que a linha 6 do step **Acessando variaveis de ambiente** está com o valor em branco. Isso deve-se ao fato que, embora a variavel tenha sido definida, ela não foi escrita no arquivo GITHUB_ENV.

Se quisermos utilizar essa variável definida no step **Exportando variavel** em um outro job, precisamos exportá-la também para GITHUB_ENV. 

https://github.com/guilhermercandido/repo_testes/blob/e8e594313e30504f3eb58e5c2cb46f49dfaf3b0b/.github/workflows/1-teste.yml#L21-L42

![img_002](https://github.com/guilhermercandido/repo_testes/assets/26474513/e4ae5191-5a5c-4666-8d5d-f80fc7107674)

No exemplo acima, temos um fato importante: repare que, no segundo step, temos destacado uma seção chamada **env**. Ela nos indicará quais variáveis de ambiente customizadas estarão disponíveis para o step consultar. Enquanto no primeiro step não temos a mesma seção e o comando para exibir ${{ env.teste }}, retornou branco.

