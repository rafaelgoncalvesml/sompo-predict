# Sompo Predict — Challenge FIAP × SOMPO Seguros

> Solução de IoT + IA para transformar o seguro de maquinário agrícola
> de reativo em preventivo, antecipando falhas e riscos no campo.

Desafio real proposto pela **SOMPO Seguros** no programa Challenge da FIAP,
desenvolvido em squad de 5 integrantes ao longo de 3 sprints (2026).
🔗 Repositório oficial do squad: https://github.com/CharlotteDxD/Projeto_Somp_T1_Squad

**Meu papel: Scrum Master + frente de ML/Deep Learning**

---

## O problema

A SOMPO Seguros opera seguros de maquinário agrícola de forma reativa:
o sinistro acontece, o seguro paga. O objetivo do Challenge foi desenvolver
uma solução que antecipe o risco antes da falha, usando dados operacionais
da apólice e telemetria de campo.

## Arquitetura da solução

| Componente | O que faz | Stack |
|---|---|---|
| 🌡️ Edge/IoT | Telemetria com ESP32 + sensor MPU-6050 via MQTT | C++ (squad) |
| 🧠 ML/DL | Score de risco sobre dados SUSEP + INMET | Python, Scikit-learn, Keras (minha frente) |
| ☁️ Backend | API Flask com JWT, deploy AWS EC2 | Flask, AWS (squad) |

## Minhas entregas (ML/DL)

### Sprint 1 — EDA + Explicabilidade
- Análise exploratória dos dados SUSEP (apólices de seguro rural)
- Primeiros insights de risco por UF e ramo (Agrícola 0621 com sinistralidade acima da média)
- Prototipagem de explicabilidade com SHAP

### Sprint 2 — Modelagem

**Dataset:** 149 apólices · 7 atributos operacionais · dados SUSEP/S_RURAL + INMET

**ML (Decision Tree, max_depth=5):**
- EDA com 3 gráficos e insights de negócio validados contra o Loss Ratio real
- Tratamento de desbalanceamento testado empiricamente (class_weight vs padrão)
- Remoção explícita de data leakage antes do encoding
- Validação dupla: split 70/30 estratificado + cross-validation 5-fold

| Métrica          | Valor  |
|------------------|--------|
| Acurácia treino  | 75,0%  |
| Acurácia teste   | 26,7%  |
| Baseline ingênuo | 44,4%  |
| F1-weighted      | 0,26   |

> O modelo ficou abaixo do baseline ingênuo — e esse resultado é o insight
> mais importante da sprint. Diagnóstico: os 7 atributos operacionais
> disponíveis na apólice têm informação mútua ≈ 0 com a variável-alvo.
> O problema não é o modelo, é a ausência de sinal nas features atuais.
> Isso orienta a coleta de dados da Sprint 3: integrar a telemetria do
> sensor ESP32 como feature principal.

**Deep Learning (MLP · Keras/TensorFlow):**
- Rede MLP treinada sobre o mesmo dataset
- Comparação direta com o modelo clássico

### Sprint 3 — Integração IoT + modelo final
📅 Prevista para o 2º semestre de 2026

## Notebooks

| Notebook | Conteúdo |
|---|---|
| `01_eda_modelagem_ml.ipynb` | EDA completa, feature engineering, Decision Tree, avaliação |
| `02_deep_learning_mlp.ipynb` | MLP com Keras, curvas de treinamento, avaliação |

## Como rodar

```bash
pip install pandas scikit-learn keras tensorflow shap matplotlib seaborn
jupyter notebook
```

Abrir os notebooks na pasta `notebooks/` em ordem numérica.

## Squad T1

5 integrantes — as frentes de IoT (ESP32/MQTT), backend (Flask/JWT)
e cloud (AWS EC2) foram desenvolvidas pelos demais membros.
Repositório completo do squad: https://github.com/CharlotteDxD/Projeto_Somp_T1_Squad
