# Projeto: 3

[![Python](https://img.shields.io/badge/-python-gray?logo=python)](https://www.python.org/)
[![Vscode](https://img.shields.io/badge/-pandas-gray?logo=pandas)](https://code.visualstudio.com/)
[![Vscode](https://img.shields.io/badge/-jupyter-gray?logo=jupyter)](https://code.visualstudio.com/)

[← Voltar](../../README.md)

## Sumário

- [Objetivo](#objetivo)
- [Instruções](#instruções)
  - [Dados](#dados)
  - [Projeto](#projeto)
- [Como Submeter o Projeto](#como-submeter-o-projeto)
- [Ajuda](#ajuda)
- [Contatos](#contatos)

## Objetivo
Passar a encontrar informações presente nos dados por meio de análises simples.

## Instruções

### Dados

1. Acessar o site do [*UCI Machine Learning Repository*](https://archive.ics.uci.edu/ml/datasets.php) e procurar pelo conjunto de dados chamado **Car Evaluation**.
2. Clicar em `Data Folder` e realizar o download dos arquivos `car.data` e `car.names`;

### Projeto

#### Parte I

1. Criar uma pasta chamada `IFCE_ICA_SEUNOME` no diretório raiz do seu Google Drive, substituindo `SEUNOME`;
    1. Criar uma subpasta chamada `Projeto 3`;
    2. Fazer o upload do arquivo `.data` e `.names` para o a pasta do projeto;
    3. Criar um novo projeto do tipo Google Colab intitulado **projeto-3-`SEUNOME`**. 
       - [Clique aqui para obter ajuda!](#ajuda)


#### Parte II

1. `[Md]`: Criar e executar a célula abaixo, substituindo `SEUNOME` e o ano da turma:

```md
# Projeto: 3
Autor: SEUMOME
Turma: XXXX.X

## Objetivo
Passar a encontrar informações presente nos dados por meio de análises simples.

## Dados
Cars é um conjunto de dados que foi desenvolvido à partir de um modelo de classificação. Este conjunto de dados pode ser usado tanto como um **problemas de classificação** quanto como um **problema de regressão**

### Colunas
| # | COLUNA | DESCRIÇÃO |
| -- | -- | -- |
| 1. | `buying` | Preço de compra |
| 2. | `maint` | Preço/valor de manutenção |
| 3. | `doors` | Número de portas |
| 4. | `persons` | Número de pessoas que o carro suporta |
| 5. | `lug_boot` | Tamanho do porta-malas |
| 6. | `safety` | Estimativa de segurança do carro |
| 7. | `class` | Classes do problema |

> ***As descrições das colunas estão sujeitas a erros**

#### Classes do conjunto de dados

| Classe | Valor |
| -- | -- | -- |
| Não aceitável | `unacc` |
| Aceitável | `acc` |
| Bom | `good` |
| Muito bom | `v-good` |

## Importação do conjunto de dados
```

2. `[Code]`: Criar e executar a célula abaixo realizando o `import` das bibliotecas `numpy` e `pandas` e montando o drive do Google:

```py
# Bibliotecas
import numpy as np
import pandas as pd

''' 
Na primeira vez que você executar esta parte do código, 
uma caixa de texto e um link devem aparecer na tela. 
Clique no link e siga até encontrar o código que você 
deve colar na caixa de texto. 
'''

# Google Drive
from google.colab import drive
drive.mount('/content/drive', force_remount=True)

# Caminho do arquivo: Substituia SEUNOME e NUMERODOPROJETO
arquivo = '/content/drive/My Drive/IFCE_IAC_SEUNOME/Projeto NUMERODOPROJETO/car.data'

# Nome das Colunas
nomes = ['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety', 'class']
```

4. `[Code]`: Criar e executar a célula abaixo, realizando a leitura do arquivo `.data` por meio do `pandas` em uma variável chamada `df`:
   - Preencha o valor de `X`

```py
# Leitura dos dados
df = pd.read_csv(arquivo, names=nomes)

# Tipos das colunas
df.dtypes

''' Todas as colunas desse conjunto de dados são do tipo X '''
```

`[Md]`: Criar e executar a célula abaixo

```md
## Observação das colunas e ajustes de seus valores
```

### Parte III

1. `[Code]`: Criar e executar a célula abaixo, para observarmos os valores existentes na coluna `doors`.
   - Veja que o único valor não numérico é o `5more` que representam os carros de 5 portas ou mais.

```py
# Leitura dos dados
df['doors'].value_counts()
```

> Este conjunto de dados deve ter seus valores transformados em numéricos para que possamos aplicar algoritmos de inteligência artificial. Existem diferentes abordagens para transformarmos os valores das colunas.

### Parte IV - Abordagem 1: Find and Replace

1. `[Md]`: Criar e executar a célula abaixo

```md
### Abordagem 1: Find and Replace
Abordagem que realiza o mapeamento dos valores
```

2. `[Code]`: Criar e executar a célula abaixo
   - O código abaixo cria um dicionário mapeando os valores **indesejados** de cada coluna para um valor **desejado**.

```py
mapa = {
   # Nome da coluna
   'doors': {
      # Valor indesejado: valor desejado
      '5more': 5
   }
}

# Visualização
df.replace(mapa)['doors'].value_counts()
```

3. `[Code]`: Criar e executar a célula abaixo:
   - Preencha o restante das informações do `mapa`.
   > **Dica**: Use a função `.value_counts()` ou `.unique()` em cada coluna para encontrar os valores possíveis de cada coluna. [Clique aqui para ajuda!](#parte-iii)

```py
mapa = {
   'buying': {},
   'maint': {},
   'doors': { '5more': 5 },
   'persons': { 'more': 5 },
   'lug_boot': {},
   'safety': {
      'low': 0,
      'med': 1,
      'high': 2
   },
   'class': {
      'unacc': 0,
      'acc': 1,
      'vgood': 2,
      'good': 3
   }   
}
```

4. `[Code]`: Criar e executar a célula abaixo:
   - Certifique-se que todas as colunas são do tipo numérico

```py
df.replace(mapa).dtypes
```

### Parte V - Abordagem 2: One Hot Encoding

1. `[Md]`: Criar e executar a célula abaixo

```md
### Abordagem 2: One Hot Encoding
Abordagem que transforma uma coluna em várias outras de valores {0, 1}. 

Digamos em uma pesquisa que exista uma pergunta sobre o sexo cujas respostas são {m, f, nr} (nr: Não respondido). Esta informação se tornará uma coluna, no nosso conjunto de dados, e vai ser entendida como `object` no pandas devi aos seus valores. A abordagem *One Hot Encoding* vai transformar a coluna `sexo` em `sexo_m`, `sexo_f` e `sexo_nr` e vai atribuir valores {0, 1} a cada uma das linhas de modo que exista apenas um 1 em cada linha. 

**Exemplo**:

| # | `sexo` | `sexo_m` | `sexo_f` | `sexo_nr` |
| -- | -- | -- | -- | -- |
| 1 | m | 1 | 0 | 0 |
| 2 | f | 0 | 1 | 0 |
| 3 | nr | 0 | 0 | 1 |
```

2. `[Code]`: Criar e executar a célula abaixo:
   - Observe o tipo e o número de colunas presentes nos dados.

```py
colunas = ['buying', 'maint', 'doors', 'persons', 'lug_boot', 'safety', 'class']

pd.get_dummies(df, columns=colunas).dtypes
```

### Parte VI
1. `[Md]`: Criar e executar a célula abaixo:
   - Pesquise sobre `dimensionalidade dos dados` e escreva um parágrafo na seção abaixo.

```md
## Dimensionalidade dos dados
```

## Como submeter o Projeto
1. Procure o botão `Compartilhar` no Google Colab e escolha `copiar link`;
2. Envie o link na tarefa do Google Salada de Aula, relacionada ao projeto.

 > **Atenção**: Execute todas as células antes de enviar o link

## Ajuda
 - [Google Colab para Iniciantes](https://medium.com/machina-sapiens/google-colab-guia-do-iniciante-334d70aad531)
 - [Markdown para Iniciantes](https://produtive.me/guia/markdown-um-guia-para-iniciantes/)
 - [Uma Introdução Simples ao Pandas](https://medium.com/data-hackers/uma-introdu%C3%A7%C3%A3o-simples-ao-pandas-1e15eea37fa1)

## Contatos
Em caso de dúvidas procure contato via:
 - Google Sala de Aula
 - Email Institucional