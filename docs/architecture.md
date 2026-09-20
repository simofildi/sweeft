# Architettura Digitale & Machine Learning

Il fitotrone di Sweeft opera come sistema cognitivo a tre livelli, sostituendo il paradigma del "controllo a regole fisse" (IF-THEN) con logiche di auto-apprendimento.

## Topologia IIoT

```mermaid
graph TB
    subgraph Edge["📡 Edge Layer — Sensori e PLC"]
        S1["Sensori ambientali\nT · UR · CO₂ · PAR multispettrale"]
        S2["Sensori idraulici\npH · EC · DO"]
        S3["Dendrometri\nmicro-variazioni diametro fusto"]
        PLC["PLC industriale\nprotocollo Modbus/TCP"]
    end
    subgraph Fog["🌫️ Fog / Gateway Layer"]
        GW["Gateway locale\nMQTT · filtraggio dati grezzi"]
        BUF["Buffer offline\ncontinuità in caso di disconnessione cloud"]
    end
    subgraph Cloud["☁️ Cloud & Analytics Layer"]
        DL[("Data Lake")]
        ML1["🎯 Reinforcement Learning\nOttimizzazione della Sintesi"]
        ML2["📈 LSTM + Computer Vision\nGolden Time Prediction"]
        DASH["📊 Dashboard operatore"]
    end

    S1 --> PLC
    S2 --> PLC
    S3 --> PLC
    PLC --> GW
    GW --> BUF
    GW --> DL
    DL --> ML1
    DL --> ML2
    ML1 -->|setpoint spettro/clima/irrigazione| PLC
    ML2 -->|alert predictive harvesting| DASH
```

## Modelli di Machine Learning

### 1. Ottimizzazione della Sintesi (Reinforcement Learning)

- **Reward function:** titolo di Miracolina per grammo di peso fresco (dato periodico HPLC dal laboratorio)
- **Spazio di azione:** micro-variazioni di spettro luminoso, temperatura, curva di irrigazione
- **Obiettivo:** identificare combinazioni multi-variate ottimali che sfuggono all'intuizione agronomica umana

### 2. Golden Time Prediction (Time-Series Forecasting)

- **Modello:** reti neurali ricorrenti (LSTM) integrate con Computer Vision
- **Input:** analisi colorimetrica RGB e multispettrale del viraggio cromatico della bacca (verde → rosso scuro), incrociata con i Gradi Giorno accumulati
- **Output:** alert di *Predictive Harvesting* con finestra di 12-24 ore prima dell'inizio della senescenza

## Principi di resilienza

- Il **gateway locale** garantisce l'autonomia del fitotrone (*mission-critical operations*) anche in caso di perdita di connettività cloud
- Il **Data Lake** conserva lo storico per il retraining periodico dei modelli
