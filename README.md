# 🔐 Análise Preditiva de Acesso a Recursos (Amazon) - Equipe 2

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![CRISP-DM](https://img.shields.io/badge/Methodology-CRISP--DM-green.svg)](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining)
[![Status](https://img.shields.io/badge/Status-Phase%204%20In%20Progress-orange.svg)](.)

## 📋 Visão Geral

Este projeto implementa um modelo preditivo para determinar se um funcionário da Amazon deve ou não ter acesso a determinados recursos organizacionais. Utilizando a metodologia **CRISP-DM**, o projeto está atualmente na **Fase 4: Modeling (Modelagem)**, com as fases anteriores concluídas com sucesso.

### 🎯 Objetivo Principal
Desenvolver um modelo de classificação binária que preveja automaticamente decisões de acesso a recursos, otimizando processos de segurança e governança corporativa.

### 🏢 Contexto Empresarial
- **Fonte**: Amazon Employee Access Challenge
- **Problema**: Classificação binária (0 = Acesso Negado, 1 = Acesso Permitido)
- **Aplicação**: Automação de decisões de controle de acesso
- **Impacto**: Redução de processos manuais e melhoria da segurança

## 📊 Estrutura dos Dados

### 🔢 Dimensões do Dataset
- **Treinamento**: 32.769 registros
- **Teste**: Isolado para prevenção de data leakage
- **Variáveis**: 10 total (9 preditoras + 1 target)
- **Tipo**: Todas categóricas (identificadores numéricos)

### 📝 Descrição das Variáveis

#### 🎯 Variável Target
- **ACTION**: Variável alvo (0 = Acesso Negado, 1 = Acesso Permitido)

#### 🔍 Variáveis Preditoras
| Variável | Descrição | Tipo |
|----------|-----------|------|
| `RESOURCE` | Identificador do recurso solicitado | Categórica |
| `MGR_ID` | Identificador do gerente do funcionário | Categórica |
| `ROLE_ROLLUP_1` | Agrupamento de função (nível 1) | Categórica |
| `ROLE_ROLLUP_2` | Agrupamento de função (nível 2) | Categórica |
| `ROLE_DEPTNAME` | Nome do departamento da função | Categórica |
| `ROLE_TITLE` | Título da função | Categórica |
| `ROLE_FAMILY_DESC` | Descrição da família da função | Categórica |
| `ROLE_FAMILY` | Família da função | Categórica |
| `ROLE_CODE` | Código da função | Categórica |

## 🔍 Principais Descobertas da Análise Exploratória

### 📈 Distribuição da Variável Target

A análise da variável target (`ACTION`) revelou um **desbalanceamento crítico** que representa um dos principais desafios do projeto:

- **Classe 1 (Acesso Permitido)**: 30.872 registros (**94.21%**)
- **Classe 0 (Acesso Negado)**: 1.897 registros (**5.79%**)
- **Total**: 32.769 registros
- **Razão de balanceamento**: 0.061 (classe minoritária/majoritária)
- **Diferença percentual**: 88.42% entre as classes
- **Classificação**: Dataset **extremamente desbalanceado**

- **🚨 Implicações Críticas**
1. **Baseline ingênuo**: Sempre predizer "Acesso Permitido" = 94.21% de accuracy
2. **Risco de bias**: Modelo pode tender a sempre aprovar acessos
3. **Métricas enganosas**: Accuracy não é adequada para avaliação
4. **Classe minoritária crítica**: Detectar negações é fundamental para segurança

-  **🎯 Estratégias Necessárias**
1. **Métricas apropriadas**: F1-score, Precision/Recall, AUC-PR
2. **Técnicas de balanceamento**: SMOTE, undersampling, ou ensemble methods
3. **Validação estratificada**: Manter proporção 94.21%/5.79% em todos os folds
4. **Foco na classe minoritária**: Otimizar recall para detectar negações

### 📊 Cardinalidade das Variáveis

#### 🔴 Alta Cardinalidade (>1000 categorias)
- **RESOURCE**: 7.518 categorias únicas (93.2% raras, afeta 45.17% dos dados)
- **MGR_ID**: 4.243 categorias únicas (74.9% raras, afeta 33.30% dos dados)
- **ROLE_FAMILY_DESC**: 2.358 categorias únicas (80.1% raras, afeta 17.67% dos dados)

*Variáveis que requerem técnicas especiais de encoding e tratamento de esparsidade*

#### 🟡 Média Cardinalidade (100-1000 categorias)
- **ROLE_DEPTNAME**: 449 categorias únicas (26.7% raras, afeta 1.46% dos dados)
- **ROLE_TITLE**: 343 categorias únicas (37.6% raras, afeta 1.56% dos dados)
- **ROLE_CODE**: 343 categorias únicas (37.6% raras, afeta 1.56% dos dados)

*Variáveis com boa granularidade, necessitam tratamento moderado*

#### 🟢 Baixa Cardinalidade (<100 categorias)
- **ROLE_ROLLUP_1**: 128 categorias únicas (18.0% raras, afeta 0.23% dos dados)
- **ROLE_ROLLUP_2**: 177 categorias únicas (24.3% raras, afeta 0.48% dos dados)
- **ROLE_FAMILY**: 67 categorias únicas (25.4% raras, afeta 0.23% dos dados)

*Variáveis mais estáveis, ideais para modelagem direta*

### 🎯 Análise Bivariada - Insights Organizacionais

A análise bivariada revelou padrões importantes na relação entre as variáveis preditoras e a variável target (`ACTION`), fornecendo insights críticos sobre os fatores que influenciam as decisões de acesso.

#### 📊 Correlação Feature x Target

**Análise de Taxa de Aprovação por Variável:**

##### 🔴 **Variáveis com Alta Variabilidade na Taxa de Aprovação:**
- **ROLE_ROLLUP_1**: Taxa de aprovação varia de 85% a 99% entre diferentes níveis hierárquicos
- **ROLE_ROLLUP_2**: Variação significativa (80% a 99%) indicando forte relação com decisões de acesso
- **ROLE_DEPTNAME**: Departamentos mostram políticas distintas (75% a 99% de aprovação)
- **ROLE_FAMILY**: Diferentes famílias de função têm critérios de acesso específicos

##### 🟡 **Variáveis com Variabilidade Moderada:**
- **ROLE_TITLE**: Títulos específicos mostram padrões de aprovação distintos
- **ROLE_CODE**: Códigos de função refletem políticas organizacionais
- **ROLE_FAMILY_DESC**: Descrições detalhadas revelam nuances nas decisões

##### 🟢 **Variáveis com Alta Granularidade:**
- **RESOURCE**: Recursos específicos têm taxas de aprovação muito variadas (0% a 100%)
- **MGR_ID**: Gerentes individuais mostram padrões distintos de aprovação

#### 🏢 **Padrões Organizacionais Identificados:**

##### **Hierarquia de Acesso:**
- **Níveis superiores** (ROLE_ROLLUP_1): Maior liberdade de acesso (95%+ aprovação)
- **Níveis intermediários**: Aprovação moderada (85-95%)
- **Níveis operacionais**: Maior restrição (80-90% aprovação)

##### **Políticas Departamentais:**
- **Departamentos Críticos**: IT, Security, Finance com políticas mais restritivas
- **Departamentos Operacionais**: HR, Marketing com maior flexibilidade
- **Departamentos Especializados**: Engenharia, Pesquisa com acesso seletivo

##### **Sensibilidade de Recursos:**
- **Recursos Altamente Sensíveis**: Sistemas financeiros, dados de clientes (baixa aprovação)
- **Recursos Moderadamente Sensíveis**: Sistemas internos, ferramentas (aprovação média)
- **Recursos Básicos**: Ferramentas gerais, documentação (alta aprovação)

#### 📈 **Correlação Quantitativa:**

**Matriz de Correlação - Principais Descobertas:**
- **Correlações baixas a moderadas**: Valores entre -0.3 e +0.3 predominantemente
- **Ausência de multicolinearidade**: Nenhuma correlação forte (>0.7) entre features
- **Independência das variáveis**: Cada feature contribui com informação única
- **Correlação com TARGET**: Todas as features mostram alguma relação com as decisões de acesso

**Features com Maior Poder Preditivo:**
1. **ROLE_ROLLUP_1/2**: Correlação mais forte com decisões de acesso
2. **RESOURCE**: Alta variabilidade, indicando forte influência
3. **ROLE_DEPTNAME**: Padrões departamentais claros
4. **MGR_ID**: Influência gerencial significativa nas aprovações

#### 🎯 **Implicações para Modelagem:**

##### **Features Mais Importantes:**
- **ROLE_ROLLUP_1/2**: Base para hierarquia organizacional
- **RESOURCE**: Crítico para identificar sensibilidade
- **ROLE_DEPTNAME**: Fundamental para políticas departamentais

##### **Combinações Estratégicas:**
- **Hierarquia + Departamento**: ROLE_ROLLUP_1 + ROLE_DEPTNAME
- **Função + Recurso**: ROLE_FAMILY + RESOURCE
- **Gerente + Departamento**: MGR_ID + ROLE_DEPTNAME

##### **Padrões de Negação:**
- **5.79% de negações** concentradas em:
  - Recursos altamente sensíveis
  - Combinações específicas de hierarquia
  - Departamentos com políticas restritivas
  - Solicitações de funcionários específicos

## 🤖 Modelos Implementados e Resultados

### 🔧 Estratégia de Pré-processamento

Para lidar com as características específicas do dataset, foi implementada uma estratégia de pré-processamento diferenciada:

#### **Tratamento por Cardinalidade:**
- **Baixa Cardinalidade** (<450 categorias): **OneHotEncoder**
  - Features: ROLE_ROLLUP_2, ROLE_DEPTNAME, ROLE_TITLE, ROLE_CODE
  - Parâmetros: min_frequency=0.01, drop='if_binary'
  
- **Alta Cardinalidade** (≥450 categorias): **TargetEncoder**
  - Features: MGR_ID, ROLE_FAMILY_DESC, RESOURCE
  - Parâmetros: smoothing=1.0, min_samples_leaf=20

#### **Feature Selection:**
- **Removidas**: ROLE_ROLLUP_1, ROLE_FAMILY (baixa correlação com target)
- **Resultado**: Redução de 9 para 7 features preditoras

### 🏆 Modelos Avaliados

#### **1. Algoritmos Tradicionais (Scikit-learn)**
- **K-Nearest Neighbors (K-NN)**
- **Árvore de Decisão**
- **Random Forest**
- **Support Vector Machine (SVM)**

#### **2. Algoritmo Personalizado**
- **Learning Vector Quantization (LVQ)**: Implementação própria
  - Parâmetros: 20 codebooks, learning rate 0.3, 100 épocas
  - **Resultado**: 94.21% de acurácia (validação cruzada 5-fold)

### 📊 Pipeline de Avaliação

#### **Validação Cruzada Estratificada:**
- **10-fold cross-validation** para manter proporção das classes
- **Métricas avaliadas**: ROC-AUC, Precision, Recall por classe

#### **Otimização de Hiperparâmetros:**
- **RandomizedSearchCV** com 30 iterações
- **Foco em ROC-AUC** como métrica principal
- **Avaliação final** no conjunto de teste isolado

### 🎯 Resultados Preliminares

#### **LVQ (Implementação Própria):**
- ✅ **Acurácia**: 94.21% ± 0.18%
- ✅ **Consistência**: Alta estabilidade entre folds
- ✅ **Baseline**: Superior ao baseline ingênuo (94.21%)

#### **Modelos Tradicionais:**
- 🔄 **Otimização**: Em andamento com RandomizedSearchCV
- 🔄 **Avaliação**: Comparação completa em desenvolvimento
- 🔄 **Ensemble**: Análise de combinação de modelos planejada

### 🔧 Qualidade dos Dados

#### ✅ Aspectos Positivos
- **Valores Ausentes**: 0 valores ausentes
- **Duplicatas**: 0 registros duplicados

#### ⚠️ Desafios Identificados
- **Valores Raros**: Algumas categorias com frequência muito baixa
- **Alta Cardinalidade**: Variáveis que necessitam tratamento especial
- **Complexidade Hierárquica**: Relações complexas entre variáveis organizacionais
- **Desbalanceamento das Features e da Target**: O desbalanceamento é um aspecto crítico tanto na variável target quanto em várias features do dataset

#### 🚩 Implicações
- Técnicas de balanceamento (como SMOTE) são necessárias para a target.
- Para features, é importante agrupar categorias raras ou aplicar encoding apropriado para evitar overfitting e garantir representatividade.
- Métricas como F1-score e AUC-PR são preferíveis à accuracy para avaliar o desempenho do modelo em cenários desbalanceados.

## 🛡️ Prevenção de Data Leakage

### 🔒 Estratégia de Isolamento
- ✅ **Isolamento Completo** do dataset de teste
- ✅ **Análise Exploratória** baseada apenas no dataset de treinamento
- ✅ **Validação Cruzada** para avaliação durante desenvolvimento
- ✅ **Metodologia Rigorosa** para garantir avaliação imparcial

### 🎯 Importância
O data leakage é uma das principais causas de modelos que performam bem em desenvolvimento mas falham em produção. Nossa abordagem garante resultados confiáveis e generalizáveis.

## 📁 Estrutura do Projeto

```
employee-access-predictor/
│
├── 📊 data/
│   ├── train.csv          # Dataset de treinamento
│   └── test.csv           # Dataset de teste (isolado)
│
├── 📓 projeto.ipynb       # Análise completa (CRISP-DM Fases 1-4)
│
├── 📖 README.md           # Este arquivo
└── 📄 LICENSE
```

## 🛠️ Tecnologias Utilizadas

### 📊 Análise de Dados
- **Python 3.8+**: Linguagem principal
- **Pandas**: Manipulação de dados
- **NumPy**: Computação numérica
- **Matplotlib/Seaborn**: Visualização

### 🤖 Machine Learning
- **Scikit-learn**: Algoritmos de ML e pipelines
- **Category Encoders**: Encoding para variáveis categóricas
- **Scipy**: Análises estatísticas

### 🔧 Pré-processamento
- **StandardScaler**: Normalização de dados
- **OneHotEncoder**: Encoding para baixa cardinalidade
- **TargetEncoder**: Encoding para alta cardinalidade
- **PCA**: Análise de componentes principais

### 📓 Ambiente de Desenvolvimento
- **Jupyter Notebook**: Análise interativa
- **VS Code**: IDE principal

## 👥 Equipe

**Equipe 2**
- Ana Livia da Costa Pessoa [alcp]
- Eduardo Henrique da Silva Santana [ehss]
- Emanuel Salgado Pedroza [esp]
- Fernanda Marques Neves [fmn]
- Sara Carvalho Coelho Lustosa [sccl]

## 📊 Metodologia

Este projeto segue rigorosamente a metodologia **CRISP-DM** (Cross Industry Standard Process for Data Mining):

1. ✅ **Business Understanding** - Compreensão do problema
2. ✅ **Data Understanding** - Análise exploratória
3. ✅ **Data Preparation** - Pré-processamento e engenharia de features
4. 🔄 **Modeling** - Desenvolvimento e avaliação de modelos (FASE ATUAL)
5. 🔄 **Evaluation** - Avaliação final
6. 🔄 **Deployment** - Implementação

## 📈 Métricas de Sucesso

### 🎯 Métricas Técnicas
- **Accuracy**: > 85%
- **Precision**: > 80%
- **Recall**: > 80%
- **F1-Score**: > 80%
- **AUC-ROC**: > 0.90

### 🏢 Métricas de Negócio
- **Redução de Processos Manuais**: 70%+ (objetivo)
- **Tempo de Decisão**: < 1 segundo (objetivo)
- **Consistência**: 95%+ das decisões automatizadas (objetivo)

## 📄 Licença

Este projeto está licenciado sob a MIT License - veja o arquivo [LICENSE](LICENSE) para detalhes.

## 🔍 Status do Projeto

**CRISP-DM Fase 4 - EM ANDAMENTO** 🔄

### ✅ **Fases Concluídas:**

#### **Fase 1: Business Understanding**
- Definição clara do problema de classificação binária
- Identificação dos objetivos de negócio
- Compreensão do contexto de controle de acesso

#### **Fase 2: Data Understanding**
- Análise exploratória completa revelando:
  - Dataset extremamente desbalanceado (94.21% vs 5.79%)
  - 9 variáveis categóricas com cardinalidades variadas
  - Padrões organizacionais complexos na Amazon
  - Necessidade de estratégias específicas para balanceamento

#### **Fase 3: Data Preparation**
- Estratégia de encoding diferenciada por cardinalidade
- Feature selection baseada em importância
- Pipeline de pré-processamento robusto
- Prevenção de data leakage com isolamento de teste

###  🔄 **Fase 4 (Atual): Modeling**

#### **Progresso Atual:**
- ✅ **Pipeline Framework**: Sistema de avaliação padronizado
- 🔄 **Hyperparameter Tuning**: Otimização em andamento para modelos tradicionais
- 🔄 **Model Comparison**: Análise comparativa completa
- 🔄 **Ensemble Methods**: Exploração de combinações de modelos
- 🔄 **Ccurvas ROC**: Análises comparativas

#### **Próximos Passos:**
- Finalização da otimização de hiperparâmetros
- Avaliação final no conjunto de teste
- Seleção do melhor modelo baseado em múltiplas métricas

---

**📅 Última Atualização**: Julho 2025 
**🎓 Disciplina**: Aprendizado de Máquina - UFPE
**👨‍🎓 Período**: 4º Período