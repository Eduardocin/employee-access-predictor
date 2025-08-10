# 🔐 Análise Preditiva de Acesso a Recursos (Amazon) - Equipe 2

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![CRISP-DM](https://img.shields.io/badge/Methodology-CRISP--DM-green.svg)](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining)
[![Status](https://img.shields.io/badge/Status-Concluído-brightgreen.svg)](.)

## 📋 Visão Geral
Projeto desenvolvido na disciplina **Aprendizado de Máquina (UFPE)** para automatizar a concessão de acesso a recursos da Amazon, substituindo o processo manual por um modelo de **classificação binária**.

O projeto foi dividido em **dois notebooks**:
- `projeto_analise.ipynb` — Etapas de **Business Understanding**, **Data Understanding** e **Data Preparation**
- `projeto_models.ipynb` — Etapas de **Modeling** e **Evaluation**
  
- **Data de Conclusão:** Julho 2025  
- **Período:** 4º período de Ciência da Computação

---

## 🎯 Objetivo
Prever se um funcionário deve receber **Acesso Permitido (1)** ou **Acesso Negado (0)** com alta precisão, aumentando **eficiência, segurança e consistência**.

---

## 📊 Dados
- **Total de registros:** 32.769  
- **Features:** 9 categóricas  
- **Target:** Extremamente desbalanceada (~94,3% aprovações / ~5,7% negações)  
- **Qualidade:** Sem valores ausentes ou duplicados

---

## ⚠️ Desafios
- Desbalanceamento extremo da classe target
- Alta cardinalidade (`RESOURCE` >7k, `MGR_ID` >4k categorias)
- Esparsidade e predominância de aprovações em categorias

---

## 🔧 Preparação
- Remoção de `ROLE_ROLLUP_1` e `ROLE_FAMILY`  
- **Codificação**: OneHotEncoder (baixa cardinalidade) / TargetEncoder (alta cardinalidade)  
- **Balanceamento**: `NeighbourhoodCleaningRule (NCR)`  
- **Escalonamento**: `StandardScaler` para modelos sensíveis à escala  

---

## 🤖 Modelagem
**Fase 1 — Modelos Tradicionais:**  
- Melhor desempenho: **Random Forest** (AUC 0.85, F1-Classe 0 = 0.420)

**Fase 2 — Modelos Avançados:**  
- Melhor desempenho: **XGBoost Otimizado** (AUC 0.83, 170 acertos na Classe 0)

---

## 🏆 Modelo Final
📌 **XGBoost Otimizado**  
- Melhor acerto na classe minoritária  
- Perfil de risco mais seguro  
- Aplicação: Aprovação automática de solicitações de baixo risco + revisão humana para casos críticos

---

## 📈 Métricas no Teste
| Modelo         | AUC  | F1-Classe 0 | Acertos Classe 0 |
|----------------|------|-------------|------------------|
| **XGBoost**    | 0.83 | 0.42        | **170**          |
| Random Forest  | 0.85 | 0.420       | 145              |

---

## 📁 Estrutura
```
employee-access-predictor/
│
├── data/
│ ├── train.csv
│ └── test.csv
│
├── projeto_analise.ipynb 
├── projeto_models.ipynb 
├── README.md
└── LICENSE
```

---

## 🛠️ Tecnologias
- Python 3.8+
- Pandas, NumPy
- Matplotlib, Seaborn
- Scikit-learn, Category Encoders
- XGBoost, LightGBM
- Jupyter Notebook

---

## 🚀 Próximos Passos
1. Coletar dados mais recentes
2. Criar novas features com especialistas
3. Refinar hiperparâmetros com otimizadores avançados
4. Implementar monitoramento contínuo
5. Integrar via API

---

## 👥 Equipe
- Ana Livia da Costa Pessoa (alcp)  
- Eduardo Henrique da Silva Santana (ehss)  
- Emanuel Salgado Pedroza (esp)  
- Fernanda Marques Neves (fmn)  
- Sara Carvalho Coelho Lustosa (sccl)  


