# Projeto: Otimização de Parâmetros Agrícolas

Este projeto demonstra um pipeline completo de Machine Learning para **otimizar parâmetros agrícolas** (PH, umidade, irrigação e nutrientes NPK) visando **maximizar a produção por hectare**, utilizando dados de solo e cultura. O notebook culmina em uma **interface interativa** para recomendação de manejo agrícola.

---

## 1. Importação de Bibliotecas e Carregamento de Dados
**Objetivo:** Centralizar dependências e carregar o dataset inicial.

- Importa bibliotecas essenciais: `pandas`, `gradio`, `numpy`, `xgboost`, `sklearn` e `scipy.optimize`.  
- Carrega o dataset `banco_solos_integrado_1000_linhas_v2.csv`.  
- Inspeção inicial com `head()`, `info()` e `describe()`.  
- Define limites das variáveis (`feature_bounds`) e quais colunas serão otimizadas.

---

## 2. Tratamento de Valores Ausentes
**Objetivo:** Garantir a integridade e usabilidade do dataset.

- Variáveis categóricas (`cultura`, `tipo_solo`) são preenchidas com a **moda**.  
- Variáveis numéricas são preenchidas com a **mediana**, robusta a outliers.

---

## 3. Codificação One-Hot
**Objetivo:** Transformar variáveis categóricas em formato numérico para os modelos.

- Aplica One-Hot Encoding em `cultura` e `tipo_solo` usando `pd.get_dummies()`.  
- `drop_first=True` evita multicolinearidade, garantindo estabilidade e interpretabilidade.

---

## 4. Seleção de Features e Divisão Train/Test
**Objetivo:** Preparar os dados para treinamento e avaliação do modelo.

- Define `producao_ton_ha` como variável alvo (`y`).  
- Remove colunas não preditivas (`id`) para formar `X`.  
- Divide em treino e teste (`X_train`, `X_test`, `y_train`, `y_test`) com `test_size=0.2` e `random_state=42`.

---

## 5. Treinamento e Avaliação de Modelos
**Objetivo:** Treinar e comparar múltiplos algoritmos de regressão.

- Modelos treinados:  
  - LinearRegression  
  - RandomForestRegressor  
  - GradientBoostingRegressor  
  - XGBRegressor  
- Métricas: MAE, MSE, RMSE e R².  
- Embora todos os modelos sejam avaliados, o pipeline de otimização **usa sempre RandomForest**, adequado para relações não lineares.

---

## 6. Definição do Modelo para Otimização
**Objetivo:** Selecionar e preparar o modelo usado no Gradio.

- Modelo de otimização definido explicitamente: `RandomForestRegressor`.  
- Para RandomForest, uma função de **Random Search** é criada, explorando limites das variáveis para otimização de PH, umidade, irrigação e NPK **rapidamente e sem travar**.  
- Benefícios:  
  - **Rastreabilidade:** modelo conhecido.  
  - **Reprodutibilidade:** resultados consistentes com semente fixa.  
  - **Flexibilidade:** trocar o modelo sem alterar a interface Gradio.  
  - **Uso do modelo mais adequado:** evita problemas de modelos lineares e gera recomendações realistas.

---

## 7. Interface Interativa com Gradio
**Objetivo:** Fornecer ferramenta prática para recomendação personalizada.

- Função `otimizar_parametros` recebe `cultura` e `tipo_solo`.  
- Ajusta automaticamente colunas one-hot correspondentes.  
- Para RandomForest, chama **Random Search** para determinar PH, umidade, irrigação e nutrientes ideais.  
- Retorna também a **produção máxima prevista**.  
- Interface permite seleção via dropdowns e exibe resultados em textboxes.  
- Mantém flexibilidade para alterar o modelo sem modificar a interface.

---

## Conclusão
O projeto demonstra a aplicação prática de Machine Learning na agricultura, transformando **dados brutos em insights acionáveis**.  

- Combina **modelagem preditiva, otimização e interface interativa**.  
- RandomForest + Random Search garante recomendações **realistas e rápidas**, mesmo com dataset não linear.  
- Gradio democratiza o acesso às recomendações, tornando a IA uma ferramenta estratégica para **maximizar produtividade e sustentabilidade** das lavouras.
