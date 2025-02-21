# Comparação de Métricas de Classificação e Regressão com Armazenamento em Banco de Dados

**OBJETIVO :** Desenvolver um sistema para experimentação e comparação de modelos de Machine Learning (ML) que armazene os modelos, hiperparâmetros, estratégias de pré-processamento e métricas de desempenho em um banco de dados. O sistema permitirá o recalculo e geração de relatórios comparativos de desempenho dos modelos treinados.

## Preparação do Ambiente

### Ambiente Virtual
Criação:
```python
    python -m venv ven
```
Ativação:
```python
    venv\Scripts\Activate
```

### Instalação das bibliotecas 
Para a execução correta é necessário a instalação das bibliotecas a seguir:

```python
    pip install numpy pandas scikit-learn matplotblib seaborn
```

```python
    pip install sqlalchemy
```

## Estrutura do Banco de Dados  

### Models (Modelos de ML)  
Armazena os modelos utilizados nos experimentos.  
- `id` (PK) → Identificador único do modelo.  
- `algorithm` → Algoritmo de ML (ex: RandomForest, SVM, DecisionTree).  
- `type` → Tipo de modelo: **0** (Classificação) | **1** (Regressão).  

### Hyperparameters (Hiperparâmetros)  
Guarda os hiperparâmetros utilizados nos modelos.  
- `id` (PK) → Identificador único do hiperparâmetro.  
- `model_id` (FK) → Referência ao modelo associado.  
- `name` → Nome do hiperparâmetro (ex: learning rate, número de árvores).  
- `value` → Valor do hiperparâmetro.  

### LearnStrategies (Estratégias de Aprendizado)  
Registra estratégias de pré-processamento e divisão dos dados.  
- `id` (PK) → Identificador único da estratégia.  
- `model_id` (FK) → Referência ao modelo associado.  
- `preprocessing_type` → Tipo de pré-processamento: **0** (Scaling) | **1** (Sampling) | **2** (Feature Selection).  
- `data_sampling` → Técnica de amostragem: **Undersampling**, **Oversampling**, **Stratified Sampling**.  
- `type` → Tipo de divisão: **0** (Cross-Validation) | **1** (Hold-out).  
- `train_size` → Percentual dos dados para treino.  
- `validation_size` → Percentual dos dados para validação.  
- `test_size` → Percentual dos dados para teste.  

### Metrics (Métricas)  
Armazena os resultados dos experimentos com base no modelo e estratégia aplicados.  
- `id` (PK) → Identificador único da métrica.  
- `strategy_id` (FK) → Referência à estratégia utilizada.  
- `model_type` → Tipo do modelo (Classificação ou Regressão).  
- `type` → Tipo da métrica calculada:  
  - **Classificação:** **0** (Acurácia) | **1** (Precisão) | **2** (Recall) | **3** (F1-score) | **4** (AUC-ROC).  
  - **Regressão:** **0** (MSE) | **1** (RMSE) | **2** (MAE) | **3** (R²).  
- `value` → Valor da métrica.  

### Experiments (Experimentos)  
Registra os experimentos realizados, associando modelos, estratégias e datasets.  
- `id` (PK) → Identificador único do experimento.  
- `model_id` (FK) → Referência ao modelo utilizado.  
- `strategy_id` (FK) → Referência à estratégia de aprendizado usada.  
- `dataset` → Nome do dataset usado no experimento (ex: Iris, Breast Cancer, Boston Housing).  
- `date_created` → Data de criação do experimento (padrão: `CURRENT_TIMESTAMP`).  
- `date_updated` → Data da última atualização (atualiza automaticamente).  
