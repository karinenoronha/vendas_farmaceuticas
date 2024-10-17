# ANÁLISE DE VENDAS FARMACÊUTICAS

## Compreender as tendências, padrões e características das vendas farmacêuticas em diferentes regiões e demografias.
Esta é uma introdução de como analisar dados de vendas e índices  Python. A fonte utilizada foi o PORTAL DE DADOS ABERTO do Governo Federal
(https://dados.gov.br/dados/conjuntos-dados/venda-de-medicamentos-controlados-e-antimicrobianos---medicamentos-industrializados), 
por onde é possível obter os dados de forma muito simples. Este estudo não tem como finalidade a recomendação de compra de nenhum medicamento, é somente uma demonstração de como utilizar a linguagem Python para começar 
uma análise de vendas.

A importação dos dados é feita de forma muito simples, foi feito o download dos dados pelo site do portal de dados aberto e adicionado no drive. Ao entrar no link clicar em “recursos” e buscar pelo dado:

“Medicamentos Industrializados - Novembro de 2021”

## Tabela de Conteúdos
- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Análise](#análise)
  - [Passo 1: Limpeza de Dados](#passo-1-limpeza-de-dados)
  - [Passo 2: Análise Exploratória](#passo-2-análise-exploratória)
  - [Passo 3: Visualizações](#passo-3-visualizações)
- [Resultados](#resultados)
- [Conclusão](#conclusão)

## Tecnologia e bibliotecas Utilizadas
- Python
- Pandas
- Matplotlib
- Seaborn

## Análise
### Passo 1: Limpeza de dados
A primeira etapa foi inspecionar o conjunto de dados para entender sua estrutura e identificar a presença de valores nulos e dados inconsistentes.
Para identificar a quantidade de valores nulos em cada coluna do DataFrame, criamos uma função específica que retorna um resumo dos metadados, incluindo a contagem de nulos

  ```
  def generate_metadata(dataframe):
      """
      Gera um dataframe contendo metadados das colunas do dataframe fornecido.
  
      :param dataframe: DataFrame para o qual os metadados serão gerados.
      :return: DataFrame contendo metadados.
      """
  
   metadata = pd.DataFrame(
          {
              'nome_variavel': dataframe.columns,
              'tipo': dataframe.dtypes,
              'qt_nulos': dataframe.isnull().sum(),
              'percent_nulos': round((dataframe.isnull().sum() / len(dataframe)) * 100, 2),
              'cardinalidade': dataframe.nunique()
          }
      )
  
      metadata = metadata.sort_values(by='tipo')
      metadata = metadata.reset_index(drop=True)
  
      return metadata
 ```

# Passo 2: Análise Exploratória
Começamos analisando as primeiras e as últimas cinco linhas do DataFrame para ter uma visão geral da estrutura dos dados.
Essa etapa inicial nos permitiu identificar rapidamente possíveis problemas, como a formatação inadequada ou dados ausentes.Para obter uma compreensão mais
 profunda das variáveis presentes no conjunto de dados, criamos uma função específica que exibe os tipos de dados de cada coluna. Isso nos ajudou a identificar se as colunas estavam no formato correto para análises subsequentes.
Após compreender os tipos de dados, utilizamos a função describe() do Pandas para gerar estatísticas descritivas das variáveis numéricas. Essa função nos proporcionou uma visão geral das principais métricas,
como média, mediana, desvio padrão, valores mínimo e máximo, e quantis.

 ```df_vendas_farma_oco_01.describe() ```
 ![image](https://github.com/user-attachments/assets/f2bc2a62-df88-48ab-b25a-4fcc2198fcb6)


 # Passo 3: Visualizações.
 Criamos alguns gráficos para visualizarmos melhor as correlações entre as variáveis. Entre eles, o gráfico de barras frequencia de vendas x UF de cada estado.
``` metadados = []
    for coluna in df.columns:
        metadados.append({
            'Variável': coluna,
            'Tipo': df[coluna].dtype,
            'Cardinalidade': df[coluna].nunique()
        })

    df_metadados = pd.DataFrame(metadados)

    # Filtra colunas com cardinalidade maior que o corte e tipos não numéricos
    variaveis_categoricas = df_metadados[(df_metadados['Cardinalidade'] <= corte_cardinalidade) & (df_metadados['Tipo'] == 'object')]

    # Calcula o número de linhas e colunas para os subplots
    n_linhas = -(-len(variaveis_categoricas) // graficos_por_linha)  # Ceiling division
    n_colunas = min(len(variaveis_categoricas), graficos_por_linha)

    # Plota as variáveis categóricas
    fig, axs = plt.subplots(nrows=n_linhas, ncols=n_colunas, figsize=(15, 5 * n_linhas))

    for i, (idx, linha) in enumerate(variaveis_categoricas.iterrows()):
        var = linha['Variável']
        ax = axs[i // graficos_por_linha, i % graficos_por_linha]
        df[var].value_counts().sort_index().plot(kind='bar', ax=ax, color='skyblue')
        ax.set_title(f'Frequência em {var}')
        ax.set_ylabel('Frequência')
        ax.set_xlabel(var)

    # Remove os eixos vazios, se houver
    for j in range(i + 1, n_linhas * n_colunas):
        axs[j // graficos_por_linha, j % graficos_por_linha].axis('off')

    plt.tight_layout()
    plt.show()

# Testa a função com os dados e o corte padrão de cardinalidade
plot_categorical_frequency_pt(df_vendas_farma_oco_01, corte_cardinalidade=30, graficos_por_linha=2)
```

![image](https://github.com/user-attachments/assets/83f6f9bf-2224-4c44-b505-11513d2efae1)



#  Resultados
1.Quantidade de observações: Existem 200.000 registros no conjunto de dados. Isso indica uma quantidade considerável de vendas do produto.
2. Estado e cidade com mais vendas: O Estado que mais vendeu foi o Rio de Janeiro (70.898 registros) .
3.Cidade mais vendas: capital do estado Rio de Janeiro (15.459 registros)
4.Princípio ativo mais vendido: A AZITROMICINA DI-HIDRATADA é o mais vendido (12.615 registros), o que pode indicar que os médicos prescrevem este remédio com muita frequencia, principalmente, no tempo do COVID-19
5. Descrição de apresentação: O tipo descrito mais frequente é 500 MG CAP DURA CT BL AL PLAS TRANS X 21 (5.890 registros)
6. A quantidade média que é solicitada por comprador é 4(Arredondando)



