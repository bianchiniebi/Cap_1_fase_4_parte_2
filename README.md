# README do Projeto: Otimização de Parâmetros Agrícolas

Este projeto demonstra um pipeline completo de Machine Learning para otimizar parâmetros agrícolas (PH, umidade, irrigação, e nutrientes NPK) visando maximizar a produção por hectare, utilizando dados de solo e cultura. O notebook culmina em uma interface interativa para recomendação de manejo agrícola.

## 1. Importação de Bibliotecas e Carregamento de Dados

**Objetivo**: Centralizar as dependências e carregar o dataset inicial.

- Importa bibliotecas essenciais como `pandas`, `gradio`, `numpy`, `xgboost`, `sklearn` (para modelagem e pré-processamento) e `scipy.optimize` (para otimização).
- Carrega o dataset `banco_solos_integrado_1000_linhas_v2.csv`.
- Realiza uma inspeção inicial (`head()`, `info()`, `describe()`) para entender a estrutura dos dados, tipos de variáveis e identificar valores ausentes.

## 2. Tratamento de Valores Ausentes

**Objetivo**: Preencher dados faltantes para garantir a integridade e usabilidade do dataset por algoritmos de Machine Learning.

- Variáveis categóricas (`cultura`, `tipo_solo`) são preenchidas com a **moda**.
- Variáveis numéricas são preenchidas com a **mediana**, escolhida por ser mais robusta a outliers em comparação com a média.

## 3. Codificação One-Hot

**Objetivo**: Converter variáveis categóricas em um formato numérico compreensível pelos modelos de Machine Learning.

- Aplica One-Hot Encoding nas colunas `cultura` e `tipo_solo` usando `pd.get_dummies()`.
- Utiliza `drop_first=True` para evitar multicolinearidade, tornando o modelo mais estável e interpretável.

## 4. Seleção de Features e Divisão Train/Test

**Objetivo**: Preparar os dados para o treinamento e avaliação do modelo.

- Define `producao_ton_ha` como a variável alvo (`y`).
- Remove colunas não preditivas (`id` e a coluna alvo) para formar o conjunto de features (`X`).
- Divide o dataset em conjuntos de treino e teste (`X_train`, `X_test`, `y_train`, `y_test`) usando `train_test_split` com `test_size=0.2` e `random_state=42` para reprodutibilidade.

## 5. Treinamento e Avaliação de Modelos

**Objetivo**: Treinar e comparar o desempenho de múltiplos algoritmos de regressão.

- Quatro modelos são treinados:
    - `LinearRegression`
    - `RandomForestRegressor`
    - `GradientBoostingRegressor`
    - `XGBRegressor`
- Para cada modelo, são calculadas as seguintes métricas de desempenho no conjunto de teste:
    - MAE (Mean Absolute Error)
    - MSE (Mean Squared Error)
    - RMSE (Root Mean Squared Error)
    - R² (R-squared)

## 6. Identificação do Melhor Modelo

**Objetivo**: Selecionar o modelo com o melhor desempenho para a etapa de otimização.

- O modelo com o maior valor de R² é identificado como o melhor, pois este coeficiente indica a proporção da variância da variável dependente que é explicável pelas variáveis independentes do modelo.
- O modelo selecionado (`best_model`) é armazenado para uso posterior.

## 7. Otimização de Parâmetros para Sugestão

**Objetivo**: Utilizar o modelo treinado para encontrar os parâmetros agrícolas ideais que maximizam a produção.

- Realiza uma **modelagem inversa**: ao invés de prever a produção, busca os inputs que maximizam a previsão de produção.
- Define as features de otimização (`ph`, `umidade_pct`, `irrigacao_horas`, `n_mgkg`, `p_mgkg`, `k_mgkg`).
- Define limites de otimização baseados nos valores mínimo e máximo observados no conjunto de treino (`feature_bounds`).
- Utiliza `scipy.optimize.minimize` com o método 'L-BFGS-B' para encontrar os valores ótimos. A função objetivo é o negativo da previsão do modelo para maximizar a produção.
- Exibe a produção máxima prevista e os valores ideais para cada parâmetro.

## 8. Interface Interativa com Gradio

**Objetivo**: Fornecer uma ferramenta prática e interativa para usuários finais, permitindo a seleção de cultura e tipo de solo para otimização personalizada.

- Uma função `otimizar_parametros` é criada, que aceita `cultura` e `tipo_solo` como entradas.
- Dentro desta função, a lógica de otimização é aplicada, ajustando as variáveis 'one-hot' correspondentes à cultura e tipo de solo selecionados.
- Os valores ideais de PH, umidade, irrigação e nutrientes NPK, juntamente com a produção máxima estimada, são retornados.
- Uma interface `gradio.Interface` é construída, permitindo aos usuários selecionar a cultura e o tipo de solo via dropdowns e visualizar os parâmetros ótimos em textboxes.
- A interface é lançada localmente, transformando o notebook em um protótipo de sistema de recomendação agrícola.

## Conclusão

Este projeto demonstra a aplicação prática de Machine Learning na agricultura, transformando dados brutos em insights acionáveis. Ao prever e otimizar as condições de cultivo, o modelo oferece uma ferramenta valiosa para agrônomos e produtores, permitindo a tomada de decisões mais informadas para maximizar a produtividade e a sustentabilidade das lavouras. A interface interativa com Gradio democratiza o acesso a essas recomendações, tornando a tecnologia de IA uma aliada estratégica no campo.
