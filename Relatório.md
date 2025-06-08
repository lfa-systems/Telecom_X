# TelecomX Brasil
# Relatório de Análise de Evasão de Clientes (Churn)

## 1. Introdução
Este projeto apresenta uma análise aprofundada do problema de evasão de clientes, conhecido como "Churn", na empresa do setor de telecomunicação. O objetivo principal deste estudo é identificar os fatores que levam os clientes a cancelar seus serviços e, com base nesses insights (percepções), propor estratégias eficazes para a retenção de clientes. A evasão de clientes representa um desafio significativo para as empresas de telecomunicações, impactando diretamente a receita e a sustentabilidade do negócio. Compreender os motivos por trás do Churn é crucial para desenvolver ações proativas e personalizadas que visem aumentar a satisfação e a fidelidade do cliente.

## 2. Limpeza e Tratamento de Dados
A primeira etapa do trabalho envolveu a importação e o tratamento do conjunto de dados de clientes. Para garantir a qualidade e a confiabilidade das análises, foram realizados os seguintes passos:

**Carregamento dos Dados:** Os dados foram carregados a partir de um arquivo JSON, presumivelmente contendo informações demográficas dos clientes, detalhes de serviço, comportamento de uso e o status de Churn.

**Inspeção Inicial:** Foi realizada uma inspeção inicial para verificar a estrutura dos dados, tipos de variáveis e a presença de valores ausentes.

**Tratamento de Valores Ausentes:** Foram identificados e tratamos os valores ausentes, optando por estratégias como a remoção de linhas ou colunas com um alto percentual de dados faltantes, ou a imputação de valores (média, mediana, moda) para colunas com poucos dados faltantes, conforme a natureza da variável.

**Padronização e Conversão de Tipos de Dados:** Variáveis categóricas foram codificadas, e variáveis numéricas foram verificadas quanto à sua consistência e convertidas para os tipos de dados apropriados. Por exemplo, colunas como "TotalCharges", que poderiam ser importadas como objeto, foram convertidas para tipo numérico, tratando quaisquer caracteres não numéricos ou espaços em branco.

**Tratamento de Outliers (se aplicável):** Embora não detalhado explicitamente aqui, a análise de outliers (valores discrepantes) em variáveis numéricas relevantes pode ter sido realizada para evitar distorções nas análises estatísticas e nos modelos.


**Linguagem usada Python**

### Exemplo de código 

```
# Importando bibliotecas
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Definino caminho do arquivo de dados
url = 'https://raw.githubusercontent.com/lfa-systems/Telecom_X/refs/heads/main/dados/TelecomX_Data.json'
# Dicionário de dados
# https://github.com/lfa-systems/Telecom_X/blob/main/dados/TelecomX_dicionario.md

# Converter arquivo de dados para Data Frame, para a análise e manipulação de dados.
dados = pd.read_json(url)

# Verificar valores nulos
df.isna().sum()

# Verificar registros duplicados
df.duplicated().sum()

# Conhecendo valores únicos das colunas
def ValoresUnicos(dataFrame):
  for nome_coluna in df.columns:
      print(f'{nome_coluna}\n',dataFrame[nome_coluna].unique())
ValoresUnicos(df)

# Mudar tipo da coluna account_Charges.Total para números
df['account_Charges.Total'] = df['account_Charges.Total'].astype('float64')

# Verificar colunas com valores null
print(df.isnull().sum())

# Realiza a substituição dos valores
df_pt['customer_SeniorCitizen'] = df_pt['customer_SeniorCitizen'].replace({0: 'Não', 1: 'Sim'})
```


## 3. Análise Exploratória de Dados (EDA)
A fase de Análise Exploratória de Dados (EDA) foi fundamental para entender a distribuição das variáveis, identificar padrões e correlações, e visualizar as características dos clientes que evadem em comparação com os que permanecem. Foram utilizados diversos gráficos e estatísticas para explorar as seguintes dimensões:

**Distribuição do Churn:** Verificação da proporção de clientes que evadiram versus os que permaneceram, revelando a severidade do problema.

**Características Demográficas:** Análise do impacto de fatores como gênero, idade (via "SeniorCitizen"), e status de parceiro e dependente na taxa de Churn.

**Serviços Contratados:** Investigação de quais serviços (e.g., múltiplos serviços de linha, segurança online, backup online, proteção de dispositivo, suporte técnico, streaming de TV/filmes, contrato de internet) estão mais associados à evasão ou retenção. Clientes com contratos mais longos (dois anos) tendem a ter menor Churn.

**Informações de Contrato e Pagamento:** Análise de como o tipo de contrato (mensal, anual, dois anos), o método de pagamento e o valor da fatura mensal ("MonthlyCharges") e total ("TotalCharges") influenciam o Churn. Clientes com contratos mensais geralmente apresentam maior taxa de Churn.

**Visualizações-Chave:**

**Gráficos de Barras:** Para comparar a taxa de Churn entre diferentes categorias de variáveis categóricas (e.g., Gênero vs Churn, Tipo de Contrato vs Churn).

**Histogramas/Boxplots:** Para visualizar a distribuição de variáveis numéricas em relação ao Churn (e.g., MonthlyCharges vs Churn).

**Gráficos de Dispersão:** Para identificar relações entre pares de variáveis numéricas.

**Heatmaps de Correlação:** Para entender as correlações entre todas as variáveis numéricas, destacando as que possuem maior relação com o Churn.


**Linguagem usada Python**

Gráfico quantidade de clientes que deixou a empresa

```
# Rótulos para as fatias (coluna 'Churn' do Churn_counts: 'Sim', 'Não')
labels = Churn_counts['Churn'].tolist()
# Valores para as fatias (coluna 'Total' do Churn_counts: as contagens)
sizes = Churn_counts['Total'].tolist()

# Cores para as fatias (ajustadas para 'Sim' e 'Não')
# É importante que a ordem das cores corresponda à ordem dos rótulos em 'labels'
# Se 'Sim' for o primeiro na lista de labels, a cor do 'Sim' deve ser a primeira
# Para garantir, podemos ordenar ou mapear:
color_map = {'Sim': '#FF5733', 'Não': '#4CAF50'}
colors = [color_map[label] for label in labels]

# Criando a figura e os eixos
fig, ax = plt.subplots(figsize=(6, 6))

# Criando o gráfico de pizza, que será transformado em rosca
wedges, texts, autotexts = ax.pie(
    sizes,
    labels=labels,
    colors=colors,
    autopct='%1.1f%%', # Formato para exibir porcentagens (1 casa decimal)
    startangle=90,     # Começa a primeira fatia do topo
    pctdistance=0.85,  # Posição do texto da porcentagem (mais para dentro do círculo)
    wedgeprops=dict(width=0.4, edgecolor='w') # Define a largura da "rosquinha" e a borda branca
)

# Desenha um círculo branco no centro para criar o efeito de rosca
centre_circle = plt.Circle((0,0), 0.30, fc='white')
fig.gca().add_artist(centre_circle)

# Ajusta o posicionamento para garantir que o círculo seja perfeito
ax.axis('equal')

# Ajusta o tamanho da fonte das porcentagens
plt.setp(autotexts, size=12, weight="bold", color='white') # Texto das porcentagens
# Ajusta o tamanho da fonte dos rótulos
plt.setp(texts, size=12, color='black') # Texto dos rótulos ('Sim', 'Não')

# Título do Gráfico
ax.set_title('Empresa: TELECOM X\nClientes evadidos', y=1.03, fontsize=16, fontweight='bold', loc='center')

# Remover as bordas da caixa do gráfico
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['bottom'].set_visible(False)
ax.spines['left'].set_visible(False)

# Adicionar os valores absolutos dentro ou perto das fatias
for i, (label, size) in enumerate(zip(labels, sizes)):
    angle = (wedges[i].theta2 + wedges[i].theta1) / 2
    x = np.cos(np.deg2rad(angle)) * 0.90 # Ajuste o raio para a posição X
    y = np.sin(np.deg2rad(angle)) * 0.70 # Ajuste o raio para a posição Y
    # Adicionar o texto com o valor absoluto
    ax.text(x, y, str(size), ha='center', va='center', fontsize=12, color='#183861', fontweight='bold')

plt.show()
```


Gráfico dos Chrun correlaciomado com variáveis categóricas

```
# Colunas para gerar gráficos
column_titles = {
    'customer_gender': 'Gênero',
    'customer_SeniorCitizen': 'Idoso',
    'customer_Partner': 'Parceiro',
    'customer_Dependents': 'Dependentes',
    'phone_PhoneService': 'Serviço de Telefone',
    'phone_MultipleLines': 'Múltiplas Linhas',
    'internet_InternetService': 'Serviço de Internet',
    'internet_OnlineSecurity': 'Segurança Online',
    'internet_OnlineBackup': 'Backup Online',
    'internet_DeviceProtection': 'Proteção de Dispositivo',
    'internet_TechSupport': 'Suporte Técnico',
    'internet_StreamingTV': 'Streaming de TV',
    'internet_StreamingMovies': 'Streaming de Filmes',
    'account_Contract': 'Tipo de Contrato',
    'account_PaperlessBilling': 'Fatura Sem Papel',
    'account_PaymentMethod': 'Método de Pagamento'
}

# Mostrar graficos ordenado por column_titles
categorical_cols = [col for col in column_titles.keys() if col in df_pt.columns]

# Definir o número de linhas e colunas para os subplots
n_rows = 7
n_cols = 3

# Paleta de cores para os histogramas (vermelho e amarelo)
histogram_palette = ['#4CAF50', '#FF5733'] #['#FF0000', '#FFD700']

paleta_cores = ['#4CAF50', '#FF5733']

# Padronizar valor maximo das barras
max_value = 5000

fig, axes = plt.subplots(n_rows, n_cols, figsize=(n_cols * 10, n_rows * 7))
axes = axes.flatten() # facilitar a iteração axes[i] em vez de axes[0, 0]

for i, col in enumerate(categorical_cols):
    if i < len(axes): # Garante que não excedemos o número de subplots
        ax = axes[i] # Atribui o eixo atual para facilitar a leitura

        # Definir a cor de fundo do subplot
        ax.set_facecolor('#f0f0f0') # Cinza claro

        # Obter o título personalizado do dicionário
        title = column_titles.get(col, f'Churn por {col}')
        ax.set_title('Empresa: TELECOM X\n'+title, fontsize=12)

        # Meio de pagamento em barras horizontal devido suas descrições
        if col == 'account_PaymentMethod':
            sns.countplot(data=df_pt, y=col, hue='Churn', ax=ax, palette=paleta_cores) # 'y' em vez de 'x'
            #ax.set_title(title, fontsize=12)
            ax.set_ylabel(column_titles.get(col, f'{col}'), fontsize=10) # Manter o rótulo do eixo Y para este caso
            ax.set_xlabel('') # Remover rótulo do eixo X para horizontal
            ax.set_xticks([]) # Remover ticks do eixo X para horizontal
            # Valor maximo da barra
            ax.set_xlim(0, max_value)
            # Adicionar valores nas barras
            for p in ax.patches:
                width = p.get_width() # Largura da barra horizontal
                if width > 0:
                    ax.text(width, # Posição X do texto (final da barra)
                            p.get_y() + p.get_height() / 2., # Posição Y do texto (centro da barra)
                            f'{int(width)}',
                            ha='left', va='center', fontsize=9, color='black') # Alinhamento ajustado

        else:
            # Para outras colunas categóricas, use countplot
            sns.countplot(data=df_pt, x=col, hue='Churn', ax=ax, palette=paleta_cores)
            ax.set_xlabel(column_titles.get(col, f'{col}'), fontsize=10)
            ax.set_ylabel('') # Remove o rótulo do eixo Y
            ax.set_yticks([])  # Remove os valores (ticks) do eixo Y
            # Valor maximo da barra
            ax.set_ylim(0, max_value)
            for p in ax.patches:
                height = p.get_height()
                if height > 0:
                    ax.text(p.get_x() + p.get_width() / 2.,
                            height,
                            f'{int(height)}',
                            ha='center', va='bottom', fontsize=9, color='black')

        # Define o título da legenda como 'Evadido'
        ax.get_legend().set_title('Evadido')

        # Remover as linhas de borda (spines)
        ax.spines['left'].set_visible(False)
        ax.spines['right'].set_visible(False)
        ax.spines['top'].set_visible(False)
        ax.spines['bottom'].set_visible(False)


# Ocultar quaisquer subplots vazios
for j in range(i + 1, len(axes)):
    fig.delaxes(axes[j])

# Evitar que textos dos gráficos sobreponha ou saia da área visível da figura
plt.tight_layout()

# Mostrar gráfico
plt.show()
```

### Exemplo de código para EDA (ilustrativo)
```
import matplotlib.pyplot as plt
import seaborn as sns

### Configurações para os gráficos
sns.set_style("whitegrid")
plt.figure(figsize=(12, 6))

### Distribuição do Churn
plt.subplot(1, 2, 1)
sns.countplot(x='Churn', data=df)
plt.title('Distribuição de Clientes por Churn')

### Exemplo: Churn por Tipo de Contrato
plt.subplot(1, 2, 2)
sns.countplot(x='Contract', hue='Churn', data=df)
plt.title('Churn por Tipo de Contrato')
plt.tight_layout()
plt.show()

### Exemplo: Distribuição de MonthlyCharges por Churn
plt.figure(figsize=(8, 6))
sns.boxplot(x='Churn', y='MonthlyCharges', data=df)
plt.title('MonthlyCharges por Churn')
plt.show()

### Exemplo: Heatmap de Correlação (apenas para variáveis numéricas)
#### Selecionar apenas colunas numéricas
numeric_cols = df.select_dtypes(include=np.number).columns
if 'Churn' in numeric_cols:
    corr_matrix = df[numeric_cols].corr()
    plt.figure(figsize=(10, 8))
    sns.heatmap(corr_matrix, annot=True, cmap='coolwarm', fmt=".2f")
    plt.title('Heatmap de Correlação')
    plt.show()
```

## 4. Conclusões e Insights

A análise exploratória de dados revelou diversos insights importantes sobre os padrões de evasão de clientes:

**Contratos de Curto Prazo:** Clientes com contratos mensais apresentam uma taxa de Churn significativamente maior. Isso sugere que a falta de um vínculo contratual mais longo os torna mais suscetíveis a trocar de serviço.

**Serviços Adicionais:** A adesão a serviços adicionais, como segurança online, suporte técnico e proteção de dispositivo, parece estar correlacionada com uma menor taxa de Churn. Clientes que utilizam mais serviços tendem a estar mais engajados com a empresa.

**Senior Citizens:** Clientes idosos ("SeniorCitizen") podem ter um comportamento de Churn diferente, e suas necessidades específicas devem ser consideradas.

**Fatura Mensal e Pagamento:** Clientes com faturas mensais mais altas (MonthlyCharges) e aqueles que utilizam métodos de pagamento eletrônicos podem ter uma propensão maior ao Churn. Isso pode indicar uma insatisfação com o custo ou a facilidade/problemas de pagamento.

**Parceiros e Dependentes:** Clientes com parceiros e dependentes tendem a ter menor Churn, o que pode indicar que decisões de mudança de serviço em domicílios com mais pessoas são mais complexas e menos frequentes.

Esses achados são cruciais para a empresa, pois apontam para as áreas onde a intervenção pode ter maior impacto. Por exemplo, focar na retenção de clientes com contratos mensais e incentivar a adesão a serviços adicionais são estratégias promissoras.


## 5. Recomendações

Com base nas conclusões da análise, as seguintes recomendações são propostas para reduzir a taxa de evasão de clientes:

**Programas de Fidelidade e Contratos Mais Longos:**
- Oferecer descontos e benefícios exclusivos para clientes que optam por contratos anuais ou de dois anos.
- Criar programas de fidelidade que recompensem a permanência do cliente, como bônus de dados, descontos em serviços adicionais ou upgrades de planos.

**Incentivo à Adesão de Serviços Agregados:**
- Promover ativamente os serviços de valor agregado (segurança online, suporte técnico, streaming) como forma de aumentar o engajamento e a percepção de valor dos clientes.
- Pacotes de serviços mais completos podem ser uma boa estratégia para "ancorar" o cliente.

**Monitoramento Ativo de Clientes em Risco:**
- Implementar um sistema de alerta precoce para identificar clientes com alto risco de Churn (e.g., com base em comportamento de uso, histórico de reclamações, ou tipo de contrato).
- Incentivar o contato proativo com esses clientes, oferecendo suporte, esclarecendo dúvidas ou apresentando ofertas personalizadas para reverter a decisão de evadir.

**Otimização da Estrutura de Preços e Pagamento:**
- Revisar a precificação dos planos para garantir competitividade, especialmente para clientes com faturas mensais elevadas.
- Oferecer diversas opções de pagamento e garantir a clareza e facilidade do processo de faturamento.

**Comunicação Personalizada:**
- Segmentar os clientes e adaptar a comunicação com base em suas características e histórico. Por exemplo, enviar ofertas específicas para clientes sem serviços adicionais ou para aqueles com contratos próximos ao vencimento.
- Enfatizar os benefícios de permanecer com a empresa, destacando o valor dos serviços e o bom atendimento.
- Ter um canal de comunicação seguro e legitimo para que o cliente possa confiar que esta de fato falando com a empresa.

Ao implementar essas recomendações, a TelecomX Brasil estará em uma posição mais forte para reter seus clientes, reduzir a evasão e sustentar seu crescimento no mercado.


## 🤝 Contato

Se você tiver dúvidas, sugestões ou quiser discutir o projeto, sinta-se à vontade para entrar em contato:

  * **Nome:** Luciano Azevedo
  * **Email:** lucianocomputador@gmail.com
  * **LinkedIn:** [Meu Perfil LinkedIn](https://www.linkedin.com/in/luciano-devops/)
  * **GitHub:** [Meu Perfil GitHub](https://github.com/lfa-systems)

