# Regime Detection Using Unsupervised Learning on High‑Frequency Order Book and Volume Data

This repository implements an end‑to‑end pipeline for identifying distinct market regimes by applying unsupervised learning techniques to high‑frequency order book (depth20) and trade volume (aggTrade) data. Detected regimes are characterized along three dimensions:

- **Trend**: Trending vs. Mean‑Reverting  
- **Volatility**: Volatile vs. Stable  
- **Liquidity**: Liquid vs. Illiquid  

---

## Table of Contents

1. [Background](#background)  
2. [Data Description](#data-description)  
3. [Feature Engineering](#feature-engineering)  
4. [Preprocessing & Dimensionality Reduction](#preprocessing--dimensionality-reduction)  
5. [Clustering Methodology](#clustering-methodology)  
6. [Evaluation Metrics](#evaluation-metrics)  
7. [Results & Interpretation](#results--interpretation)  
8. [Repository Structure](#repository-structure)  

---

## Background

Financial markets exhibit periods of differing behavior—trends may persist for a time before reversing, volatility may spike during stress, and liquidity can ebb and flow. By segmenting market data into **regimes**, quantitative analysts and automated strategies can adapt their models and risk controls dynamically.

---

## Data Description

- **Order Book Snapshots (`depth20/`)**  
  Top 20 bid and ask levels, with both price and quantity at each depth.

- **Aggregated Trade Events (`aggTrade/`)**  
  Time‑stamped records of executed trades, including side (buy/sell) and trade size.

Both data sources are synchronized at a sub‑second resolution to capture microstructure dynamics.

---

## Feature Engineering

We construct a comprehensive feature set at each timestamp:

| Category                  | Examples                                                                 |
|---------------------------|--------------------------------------------------------------------------|
| **Liquidity & Depth**     | Spread, order‑book imbalance, microprice, cumulative depth, sloped depth |
| **Price Dynamics**        | Rolling log‑returns, short‑term volatility (10s, 30s windows)            |
| **Volume Metrics**        | Buy/sell volume imbalance, cumulative volume (10s, 30s), VWAP shift      |
| **Aggressive Trade Impact** | Trade wipe level (average book levels consumed by trades)              |

---

## Preprocessing & Dimensionality Reduction

1. **Normalization**: All features standardized (Z‑score) to unit variance.  
2. **Principal Component Analysis (PCA)**: Retain > 90 % of total variance to reduce noise and computational cost.

---

## Clustering Methodology

We apply and compare three unsupervised algorithms:

1. **K‑Means**  
   - Elbow method to determine optimal cluster count  
2. **HDBSCAN**  
   - Captures irregular cluster shapes and labels noise points  
3. **Gaussian Mixture Models (GMM)**  
   - Provides soft membership probabilities  

---

## Evaluation Metrics

- **Silhouette Score**  
- **Davies‑Bouldin Index**  
- **Visual Inspection** via t‑SNE / UMAP projections  

These metrics guide algorithm selection and parameter tuning.

---

## Results & Interpretation

Four distinct regimes emerged consistently:

| Regime ID | Characteristics                                 | Interpretation                                    |
|:---------:|-------------------------------------------------|---------------------------------------------------|
| **0**     | Mean‑Reverting, Stable, Illiquid                | Narrow range, low activity                        |
| **1**     | Trending, Volatile, Liquid                      | Strong directional moves with high depth & volume |
| **2**     | Trending, Stable, Illiquid                      | Gentle trends in thin markets                     |
| **3**     | Mean‑Reverting, Volatile, Liquid                | Choppy reversals with high trade aggression       |

- **Transition Analysis**:  
  - Volatile regimes often **follow** illiquid‑stable states.  
  - Trend exhaustion frequently **precedes** mean‑reverting phases.  

---
