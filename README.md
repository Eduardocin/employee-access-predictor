# 🔐 Análise Preditiva de Acesso a Recursos (Amazon) - Equipe 2

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![CRISP-DM](https://img.shields.io/badge/Methodology-CRISP--DM-green.svg)](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining)
[![Status](https://img.shields.io/badge/Status-Phase%202%20Complete-success.svg)](.)

## 📋 Visão Geral

Este projeto implementa um modelo preditivo para determinar se um funcionário da Amazon deve ou não ter acesso a determinados recursos organizacionais. Utilizando a metodologia **CRISP-DM**, o projeto está atualmente na **Fase 2: Data Understanding (Compreensão dos Dados)**.

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
- **Balanceamento**: Dataset relativamente balanceado
- **Padrão**: Distribuição que permite modelagem eficaz
- **Insights**: Taxa de aprovação varia significativamente por contexto organizacional

### 📊 Cardinalidade das Variáveis

#### 🔴 Alta Cardinalidade (>1000 categorias)
- Variáveis que requerem técnicas especiais de encoding
- Potencial para overfitting sem tratamento adequado

#### 🟡 Média Cardinalidade (100-1000 categorias)
- Variáveis com boa granularidade para análise
- Balanceamento entre especificidade e generalização

#### 🟢 Baixa Cardinalidade (<100 categorias)
- Variáveis mais estáveis para modelagem
- Menor risco de overfitting

### 🎯 Análise Bivariada - Insights Organizacionais

#### 📋 Variáveis Hierárquicas
- **ROLE_ROLLUP_1 vs ROLE_ROLLUP_2**: Padrões claros de aprovação baseados na hierarquia
- **Combinações Críticas**: Certas combinações têm taxas de aprovação muito baixas
- **Combinações Privilegiadas**: Outras combinações têm aprovação quase garantida

#### 🏢 Análise Departamental
- **Departamentos Restritivos**: Alguns departamentos têm políticas de acesso mais rigorosas
- **Departamentos Liberais**: Outros têm maior flexibilidade de acesso
- **Padrões de Recursos**: Diferentes departamentos acessam recursos distintos

### 🔧 Qualidade dos Dados

#### ✅ Aspectos Positivos
- **Valores Ausentes**: 0 (zero) valores ausentes
- **Duplicatas**: 0 (zero) registros duplicados
- **Consistência**: Dados estruturados e consistentes

#### ⚠️ Desafios Identificados
- **Valores Raros**: Algumas categorias com frequência muito baixa
- **Alta Cardinalidade**: Variáveis que necessitam tratamento especial
- **Complexidade Hierárquica**: Relações complexas entre variáveis organizacionais

## 🛡️ Prevenção de Data Leakage

### 🔒 Estratégia de Isolamento
- ✅ **Isolamento Completo** do dataset de teste
- ✅ **Análise Exploratória** baseada apenas no dataset de treinamento
- ✅ **Validação Cruzada** para avaliação durante desenvolvimento
- ✅ **Metodologia Rigorosa** para garantir avaliação imparcial

### 🎯 Importância
O data leakage é uma das principais causas de modelos que performam bem em desenvolvimento mas falham em produção. Nossa abordagem garante resultados confiáveis e generalizáveis.

## 🔮 Hipóteses para Modelagem

### 🏗️ Hipóteses Principais
1. **Hierarquia Organizacional**: Diferentes níveis hierárquicos influenciam significativamente as decisões de acesso
2. **Políticas Departamentais**: Departamentos têm políticas de acesso específicas e distintas
3. **Sensibilidade de Recursos**: Recursos diferentes têm critérios de acesso variados
4. **Influência Gerencial**: O gerente do funcionário impacta nas decisões de aprovação
5. **Combinações Funcionais**: Combinações específicas de função e departamento são determinantes

### 🎯 Estratégias de Feature Engineering
- **Variáveis Derivadas**: Criar features baseadas em combinações hierárquicas
- **Agrupamentos**: Agrupar categorias raras para reduzir overfitting
- **Encoding Avançado**: Implementar técnicas específicas para alta cardinalidade

## 📁 Estrutura do Projeto

```
employee-access-predictor/
│
├── 📊 data/
│   ├── train.csv          # Dataset de treinamento
│   └── test.csv           # Dataset de teste (isolado)
│
├── 📓 projeto.ipynb       # Análise exploratória (CRISP-DM Fase 2)
│
├── 📋 Documentos/
│   ├── crisp-dm-phase-2.png
│   └── exercicio_crisp_dm_phase_2.pdf
│
├── 📖 README.md           # Este arquivo
└── 📄 LICENSE
```

## 🚀 Próximos Passos

### 🎯 CRISP-DM Fase 3: Data Preparation
- [ ] Limpeza e pré-processamento dos dados
- [ ] Tratamento de valores raros e alta cardinalidade
- [ ] Feature engineering baseado nos insights descobertos
- [ ] Encoding de variáveis categóricas
- [ ] Divisão estratificada para validação

### 🤖 CRISP-DM Fase 4: Modeling
- [ ] Seleção de algoritmos candidatos
- [ ] Implementação de baseline simples
- [ ] Modelos avançados (ensemble, deep learning)
- [ ] Otimização de hiperparâmetros
- [ ] Validação cruzada rigorosa

### 📈 CRISP-DM Fase 5: Evaluation
- [ ] Métricas de classificação (accuracy, precision, recall, F1-score)
- [ ] Análise de curva ROC e AUC
- [ ] Interpretabilidade do modelo
- [ ] Validação no dataset de teste

## 🛠️ Tecnologias Utilizadas

### 📊 Análise de Dados
- **Python 3.8+**: Linguagem principal
- **Pandas**: Manipulação de dados
- **NumPy**: Computação numérica
- **Matplotlib/Seaborn**: Visualização

### 🤖 Machine Learning
- **Scikit-learn**: Algoritmos de ML
- **Scipy**: Análises estatísticas

### 📓 Ambiente de Desenvolvimento
- **Jupyter Notebook**: Análise interativa
- **VS Code**: IDE principal

## 👥 Equipe

**Equipe 2** - Disciplina de Aprendizado de Máquina
- Universidade Federal de Pernambuco (UFPE)
- 4º Período

## 📊 Metodologia

Este projeto segue rigorosamente a metodologia **CRISP-DM** (Cross Industry Standard Process for Data Mining):

1. ✅ **Business Understanding** - Compreensão do problema
2. ✅ **Data Understanding** - Análise exploratória (FASE ATUAL)
3. 🔄 **Data Preparation** - Pré-processamento
4. 🔄 **Modeling** - Desenvolvimento do modelo
5. 🔄 **Evaluation** - Avaliação
6. 🔄 **Deployment** - Implementação

## 📈 Métricas de Sucesso

### 🎯 Métricas Técnicas
- **Accuracy**: > 85%
- **Precision**: > 80%
- **Recall**: > 80%
- **F1-Score**: > 80%
- **AUC-ROC**: > 0.90

### 🏢 Métricas de Negócio
- **Redução de Processos Manuais**: 70%+
- **Tempo de Decisão**: < 1 segundo
- **Consistência**: 95%+ das decisões automatizadas

## 📄 Licença

Este projeto está licenciado sob a MIT License - veja o arquivo [LICENSE](LICENSE) para detalhes.

## 🔍 Status do Projeto

**CRISP-DM Fase 2 - CONCLUÍDA** ✅

A análise exploratória dos dados foi finalizada com sucesso, revelando insights importantes sobre:
- Estrutura organizacional da Amazon
- Padrões de acesso baseados em hierarquia
- Desafios de modelagem (alta cardinalidade, valores raros)
- Estratégias para as próximas fases

---

**📅 Última Atualização**: Julho 2025  
**🎓 Disciplina**: Aprendizado de Máquina - UFPE  
**👨‍🎓 Período**: 4º Período