# BinBot Pro — Guia de Instalação e Uso

## Arquivos do projeto
```
server.py    ← Backend Python (conecta à Quotex via PyQuotex)
painel.html  ← Painel visual (abrir no navegador)
```

---

## Pré-requisitos
- Python 3.10+
- Conta na Quotex (demo ou real)

---

## 1. Instalar dependências

```bash
pip install pyquotex websockets pandas pandas-ta
```

---

## 2. Rodar o servidor

### Modo DEMO (sem conta — dados simulados)
```bash
python server.py
```

### Modo conectado à sua conta Quotex
```bash
# Conta demo
python server.py --email lucasbastossp@hotmail.com --password 6471Gio! --mode demo

# Conta real (cuidado!)
python server.py --email seu@email.com --password suasenha --mode real
```

---

## 3. Abrir o painel

Abra o arquivo `painel.html` direto no navegador (Chrome ou Edge recomendados).  
O painel conecta automaticamente ao servidor em `ws://localhost:8765`.

---

## Como funciona

```
Quotex WebSocket
      ↓
  server.py  (PyQuotex + cálculo de indicadores)
      ↓  WebSocket local ws://localhost:8765
  painel.html  (gráfico de velas + sinais)
```

1. O servidor conecta à Quotex, baixa candles históricos e mantém stream em tempo real.
2. Quando você clica **"GERAR SINAL"** no painel:
   - O painel envia `{"action":"request_signal", "asset":"EURUSD", "tf":"1m"}` ao servidor
   - O servidor calcula RSI, MACD, Bollinger, Médias e Estocástico
   - Retorna o sinal (CALL ou PUT) com % de confiança e motivos
3. Os preços atualizam em tempo real via ticks do WebSocket.

---

## Indicadores utilizados

| Indicador | Parâmetros | Sinal de CALL | Sinal de PUT |
|-----------|-----------|--------------|-------------|
| RSI | 14 períodos | < 30 (sobrevenda) | > 70 (sobrecompra) |
| MACD | 12, 26, 9 | Histograma positivo | Histograma negativo |
| Bollinger Bands | 20, 2σ | Toque banda inferior | Toque banda superior |
| Médias Móveis | 9 / 21 EMA | MA9 acima MA21 | MA9 abaixo MA21 |
| Estocástico | 14, 3, 3 | K < 20 | K > 80 |

---

## ⚠️ Aviso de risco

Opções binárias envolvem risco elevado de perda.  
**Sempre use conta demo antes de operar com dinheiro real.**  
Este software é educacional — o desenvolvedor não se responsabiliza por perdas.
