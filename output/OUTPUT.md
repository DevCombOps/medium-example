# Utilizando Github Actions *output*

## Introdução

Conforme os *workflows* são construidos e vão se tornando mais complexos, há a necessidade de compartilhar informações entre *steps*, *jobs* e até mesmo *workflows*. Uma das formas mais comuns de compartilhar dados é utilizando Github *outputs*, devido a sua simplicidade e por ser um recurso nativo dos *workflows*.

Esse material tem como intuito explorar a utilização dos *outputs*, suas limitações e melhores práticas de uso. Por mais que seja simples devemos tomar cuidado em alguns pontos.

## *Output* entre *steps*

A sintaxe para gerar um *output* segue o seguinte padrão:

`echo "<nome_do_output>=<valor_do_output>" >> $GITHUB_OUTPUT`

Um dos exemplos mais simples é o compartilhamento entre *steps* do mesmo job.

https://github.com/DevCombOps/medium-example/blob/b1940ba0957920cff400f62bb0b24e9cf65a74be/.github/workflows/output.yml#L1-L14

No exemplo acima temos duas *steps*, a primeira step gera o *output* e a segunda consome. A primeira step tem o atributo `id` com o valor `runner`, dessa forma qualquer outra step dentro desse job consegue consumir o *output* gerado. Podemos ver o consumo do *output* na segunda step com a seguinte sintaxe:

`${{ steps.<id_da_step_anterior>.outputs.<nome_do_output> }}`

O resultado do workflow criado será a exibição do runner name.

![resultado do workflow](imgs/output1.png)

## *Output* entre *jobs*

## *Output* entre *workflows*

## *Output* com actions/core

## O que não deve ser feito

### Sobrescrever variável GITHUB_OUTPUT

### Compartilhar secrets

### *Output* deprecated