from datetime import datetime
import json
import os
import numpy as np
import pandas as pd
import plotly.express as px
import streamlit as st

st.set_page_config(
    page_title="Kantitatif Portföy & Kasa Yönetimi",
    page_icon="📈",
    layout="wide",
)

st.markdown(
    """
    <style>
    .stApp { background-color: #0f172a; color: #f8fafc; }
    .glass-card {
        background: rgba(30, 41, 59, 0.7);
        backdrop-filter: blur(10px);
        border: 1px solid rgba(255, 255, 255, 0.1);
        border-radius: 1rem;
        padding: 1.25rem;
        margin-bottom: 1rem;
    }
    </style>
""",
    unsafe_allow_html=True,
)

DATA_FILE = "portfoy_verileri.json"

DEFAULT_TRANSACTIONS = [
    {
        "id": 1,
        "assetType": "stock",
        "symbol": "SSAAT",
        "status": "open",
        "strategy": "BIST",
        "lots": 72,
        "buyDate": "2026-07-16",
        "buyPrice": 56.0,
        "sellDate": "",
        "targetPrice": 35.8,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 2,
        "assetType": "stock",
        "symbol": "AKBNK",
        "status": "open",
        "strategy": "BIST",
        "lots": 250,
        "buyDate": "2026-07-21",
        "buyPrice": 67.28,
        "sellDate": "",
        "targetPrice": 61.6,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 3,
        "assetType": "stock",
        "symbol": "EKIM",
        "status": "open",
        "strategy": "BIST",
        "lots": 75,
        "buyDate": "2026-07-09",
        "buyPrice": 30.26,
        "sellDate": "",
        "targetPrice": 19.16,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 4,
        "assetType": "stock",
        "symbol": "VESBE",
        "status": "open",
        "strategy": "BIST",
        "lots": 1108,
        "buyDate": "2026-06-30",
        "buyPrice": 6.8,
        "sellDate": "",
        "targetPrice": 5.79,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 5,
        "assetType": "stock",
        "symbol": "FROTO",
        "status": "open",
        "strategy": "BIST",
        "lots": 78,
        "buyDate": "2026-06-17",
        "buyPrice": 90.45,
        "sellDate": "",
        "targetPrice": 76.7,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 6,
        "assetType": "stock",
        "symbol": "FROTO",
        "status": "open",
        "strategy": "BIST",
        "lots": 94,
        "buyDate": "2026-05-18",
        "buyPrice": 90.0,
        "sellDate": "",
        "targetPrice": 76.7,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 7,
        "assetType": "stock",
        "symbol": "AAGYO",
        "status": "open",
        "strategy": "BIST",
        "lots": 144,
        "buyDate": "2026-04-09",
        "buyPrice": 21.1,
        "sellDate": "",
        "targetPrice": 12.6,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 8,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "open",
        "strategy": "Altın",
        "lots": 84,
        "buyDate": "2026-03-25",
        "buyPrice": 83.58,
        "sellDate": "",
        "targetPrice": 68.11,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 9,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "open",
        "strategy": "Altın",
        "lots": 52,
        "buyDate": "2026-03-23",
        "buyPrice": 77.28,
        "sellDate": "",
        "targetPrice": 68.11,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 10,
        "assetType": "stock",
        "symbol": "GMSTRF",
        "status": "open",
        "strategy": "Silver",
        "lots": 7,
        "buyDate": "2026-03-23",
        "buyPrice": 593.25,
        "sellDate": "",
        "targetPrice": 539.75,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 11,
        "assetType": "stock",
        "symbol": "GMSTRF",
        "status": "open",
        "strategy": "Silver",
        "lots": 7,
        "buyDate": "2026-03-17",
        "buyPrice": 714.5,
        "sellDate": "",
        "targetPrice": 539.75,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 12,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "open",
        "strategy": "Altın",
        "lots": 15,
        "buyDate": "2026-03-17",
        "buyPrice": 83.59,
        "sellDate": "",
        "targetPrice": 68.11,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 13,
        "assetType": "stock",
        "symbol": "GMSTRF",
        "status": "open",
        "strategy": "Silver",
        "lots": 4,
        "buyDate": "2026-03-03",
        "buyPrice": 780.75,
        "sellDate": "",
        "targetPrice": 539.75,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 14,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "open",
        "strategy": "Altın",
        "lots": 25,
        "buyDate": "2026-03-03",
        "buyPrice": 91.54,
        "sellDate": "",
        "targetPrice": 68.11,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 15,
        "assetType": "stock",
        "symbol": "GMSTRF",
        "status": "open",
        "strategy": "Silver",
        "lots": 8,
        "buyDate": "2026-02-13",
        "buyPrice": 694.5,
        "sellDate": "",
        "targetPrice": 539.75,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 16,
        "assetType": "stock",
        "symbol": "GMSTRF",
        "status": "open",
        "strategy": "Silver",
        "lots": 15,
        "buyDate": "2026-02-02",
        "buyPrice": 660.0,
        "sellDate": "",
        "targetPrice": 539.75,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 17,
        "assetType": "stock",
        "symbol": "GMSTRF",
        "status": "open",
        "strategy": "Silver",
        "lots": 6,
        "buyDate": "2026-01-30",
        "buyPrice": 934.75,
        "sellDate": "",
        "targetPrice": 539.75,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 18,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "open",
        "strategy": "Altın",
        "lots": 4,
        "buyDate": "2026-01-26",
        "buyPrice": 90.32,
        "sellDate": "",
        "targetPrice": 68.11,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 19,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "open",
        "strategy": "Altın",
        "lots": 70,
        "buyDate": "2026-01-16",
        "buyPrice": 74.07,
        "sellDate": "",
        "targetPrice": 68.11,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 20,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "open",
        "strategy": "Altın",
        "lots": 129,
        "buyDate": "2026-01-15",
        "buyPrice": 77.96,
        "sellDate": "",
        "targetPrice": 68.11,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 21,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "open",
        "strategy": "Altın",
        "lots": 61,
        "buyDate": "2026-01-14",
        "buyPrice": 82.06,
        "sellDate": "",
        "targetPrice": 68.11,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 22,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "open",
        "strategy": "Altın",
        "lots": 60,
        "buyDate": "2026-01-13",
        "buyPrice": 86.37,
        "sellDate": "",
        "targetPrice": 68.11,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 23,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "open",
        "strategy": "Altın",
        "lots": 55,
        "buyDate": "2026-01-12",
        "buyPrice": 90.91,
        "sellDate": "",
        "targetPrice": 68.11,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 24,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "open",
        "strategy": "Altın",
        "lots": 53,
        "buyDate": "2026-01-09",
        "buyPrice": 95.69,
        "sellDate": "",
        "targetPrice": 68.11,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 25,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "open",
        "strategy": "Altın",
        "lots": 48,
        "buyDate": "2026-01-07",
        "buyPrice": 106.02,
        "sellDate": "",
        "targetPrice": 68.11,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 101,
        "assetType": "stock",
        "symbol": "METEN",
        "status": "closed",
        "strategy": "BIST",
        "lots": 96,
        "buyDate": "2026-07-28",
        "buyPrice": 20.0,
        "sellDate": "2026-07-29",
        "targetPrice": 24.2,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 102,
        "assetType": "stock",
        "symbol": "SISE",
        "status": "closed",
        "strategy": "BIST",
        "lots": 374,
        "buyDate": "2026-07-23",
        "buyPrice": 44.2,
        "sellDate": "2026-07-24",
        "targetPrice": 43.14,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 103,
        "assetType": "stock",
        "symbol": "TCELL",
        "status": "closed",
        "strategy": "BIST",
        "lots": 140,
        "buyDate": "2026-07-22",
        "buyPrice": 110.35,
        "sellDate": "2026-07-23",
        "targetPrice": 108.2,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 104,
        "assetType": "stock",
        "symbol": "SARAE",
        "status": "closed",
        "strategy": "BIST",
        "lots": 50,
        "buyDate": "2026-07-17",
        "buyPrice": 70.0,
        "sellDate": "2026-07-20",
        "targetPrice": 84.7,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 105,
        "assetType": "stock",
        "symbol": "HEKTS",
        "status": "closed",
        "strategy": "BIST",
        "lots": 5400,
        "buyDate": "2026-07-16",
        "buyPrice": 3.16,
        "sellDate": "2026-07-17",
        "targetPrice": 3.03,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 106,
        "assetType": "stock",
        "symbol": "SOKM",
        "status": "closed",
        "strategy": "BIST",
        "lots": 120,
        "buyDate": "2026-05-13",
        "buyPrice": 52.35,
        "sellDate": "2026-07-16",
        "targetPrice": 50.95,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 107,
        "assetType": "stock",
        "symbol": "SOKM",
        "status": "closed",
        "strategy": "BIST",
        "lots": 150,
        "buyDate": "2026-05-08",
        "buyPrice": 51.8,
        "sellDate": "2026-07-16",
        "targetPrice": 50.95,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 108,
        "assetType": "stock",
        "symbol": "SOKM",
        "status": "closed",
        "strategy": "BIST",
        "lots": 45,
        "buyDate": "2026-06-04",
        "buyPrice": 48.44,
        "sellDate": "2026-07-16",
        "targetPrice": 50.95,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 109,
        "assetType": "stock",
        "symbol": "SOKM",
        "status": "closed",
        "strategy": "BIST",
        "lots": 72,
        "buyDate": "2026-06-05",
        "buyPrice": 48.02,
        "sellDate": "2026-07-16",
        "targetPrice": 50.95,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 110,
        "assetType": "stock",
        "symbol": "SOKM",
        "status": "closed",
        "strategy": "BIST",
        "lots": 104,
        "buyDate": "2026-06-22",
        "buyPrice": 48.3,
        "sellDate": "2026-07-16",
        "targetPrice": 50.95,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 111,
        "assetType": "stock",
        "symbol": "ISVEA",
        "status": "closed",
        "strategy": "BIST",
        "lots": 36,
        "buyDate": "2026-07-10",
        "buyPrice": 20.9,
        "sellDate": "2026-07-14",
        "targetPrice": 27.78,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 112,
        "assetType": "stock",
        "symbol": "GOLDA",
        "status": "closed",
        "strategy": "BIST",
        "lots": 109,
        "buyDate": "2026-07-08",
        "buyPrice": 9.2,
        "sellDate": "2026-07-09",
        "targetPrice": 11.13,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 113,
        "assetType": "stock",
        "symbol": "SOHOE",
        "status": "closed",
        "strategy": "BIST",
        "lots": 67,
        "buyDate": "2026-07-06",
        "buyPrice": 15.0,
        "sellDate": "2026-07-06",
        "targetPrice": 16.4,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 114,
        "assetType": "stock",
        "symbol": "ORZAX",
        "status": "closed",
        "strategy": "BIST",
        "lots": 14,
        "buyDate": "2026-07-07",
        "buyPrice": 69.0,
        "sellDate": "2026-07-07",
        "targetPrice": 75.9,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 115,
        "assetType": "stock",
        "symbol": "BETAE",
        "status": "closed",
        "strategy": "BIST",
        "lots": 28,
        "buyDate": "2026-07-01",
        "buyPrice": 40.0,
        "sellDate": "2026-07-06",
        "targetPrice": 58.5,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 116,
        "assetType": "stock",
        "symbol": "VESBE",
        "status": "closed",
        "strategy": "BIST",
        "lots": 307,
        "buyDate": "2026-06-30",
        "buyPrice": 6.8,
        "sellDate": "2026-07-01",
        "targetPrice": 6.52,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 117,
        "assetType": "stock",
        "symbol": "VESBE",
        "status": "closed",
        "strategy": "BIST",
        "lots": 1088,
        "buyDate": "2026-06-24",
        "buyPrice": 6.25,
        "sellDate": "2026-06-30",
        "targetPrice": 7.69,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 118,
        "assetType": "stock",
        "symbol": "TAVHL",
        "status": "closed",
        "strategy": "BIST",
        "lots": 38,
        "buyDate": "2026-05-13",
        "buyPrice": 266.75,
        "sellDate": "2026-06-16",
        "targetPrice": 295.5,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 119,
        "assetType": "stock",
        "symbol": "EKDMR",
        "status": "closed",
        "strategy": "BIST",
        "lots": 27,
        "buyDate": "2026-05-22",
        "buyPrice": 45.0,
        "sellDate": "2026-06-01",
        "targetPrice": 65.8,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 120,
        "assetType": "stock",
        "symbol": "TAVHL",
        "status": "closed",
        "strategy": "BIST",
        "lots": 20,
        "buyDate": "2026-04-30",
        "buyPrice": 276.5,
        "sellDate": "2026-05-12",
        "targetPrice": 274.75,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 121,
        "assetType": "stock",
        "symbol": "SOKM",
        "status": "closed",
        "strategy": "BIST",
        "lots": 100,
        "buyDate": "2026-04-22",
        "buyPrice": 51.15,
        "sellDate": "2026-05-07",
        "targetPrice": 54.45,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 122,
        "assetType": "stock",
        "symbol": "SOKM",
        "status": "closed",
        "strategy": "BIST",
        "lots": 100,
        "buyDate": "2026-04-29",
        "buyPrice": 49.42,
        "sellDate": "2026-05-07",
        "targetPrice": 54.45,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 123,
        "assetType": "stock",
        "symbol": "TTKOM",
        "status": "closed",
        "strategy": "BIST",
        "lots": 80,
        "buyDate": "2026-04-24",
        "buyPrice": 64.2,
        "sellDate": "2026-04-28",
        "targetPrice": 63.05,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 124,
        "assetType": "stock",
        "symbol": "NUHCM",
        "status": "closed",
        "strategy": "BIST",
        "lots": 21,
        "buyDate": "2026-04-22",
        "buyPrice": 247.7,
        "sellDate": "2026-04-24",
        "targetPrice": 245.8,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 125,
        "assetType": "stock",
        "symbol": "NUHCM",
        "status": "closed",
        "strategy": "BIST",
        "lots": 21,
        "buyDate": "2026-04-14",
        "buyPrice": 244.9,
        "sellDate": "2026-04-21",
        "targetPrice": 246.9,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 126,
        "assetType": "stock",
        "symbol": "LXGYO",
        "status": "closed",
        "strategy": "BIST",
        "lots": 55,
        "buyDate": "2026-03-10",
        "buyPrice": 12.05,
        "sellDate": "2026-03-16",
        "targetPrice": 17.21,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 127,
        "assetType": "stock",
        "symbol": "MCARD",
        "status": "closed",
        "strategy": "BIST",
        "lots": 10,
        "buyDate": "2026-03-11",
        "buyPrice": 80.0,
        "sellDate": "2026-03-16",
        "targetPrice": 117.0,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 128,
        "assetType": "stock",
        "symbol": "SVGYO",
        "status": "closed",
        "strategy": "BIST",
        "lots": 174,
        "buyDate": "2026-03-09",
        "buyPrice": 3.64,
        "sellDate": "2026-03-12",
        "targetPrice": 5.85,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 129,
        "assetType": "stock",
        "symbol": "GENKM",
        "status": "closed",
        "strategy": "BIST",
        "lots": 178,
        "buyDate": "2026-03-06",
        "buyPrice": 11.0,
        "sellDate": "2026-03-11",
        "targetPrice": 16.1,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 130,
        "assetType": "stock",
        "symbol": "EMPAE",
        "status": "closed",
        "strategy": "BIST",
        "lots": 21,
        "buyDate": "2026-02-26",
        "buyPrice": 22.0,
        "sellDate": "2026-03-05",
        "targetPrice": 38.96,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 131,
        "assetType": "stock",
        "symbol": "ATATR",
        "status": "closed",
        "strategy": "BIST",
        "lots": 200,
        "buyDate": "2026-02-19",
        "buyPrice": 11.2,
        "sellDate": "2026-02-24",
        "targetPrice": 16.39,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 132,
        "assetType": "stock",
        "symbol": "BESTE",
        "status": "closed",
        "strategy": "BIST",
        "lots": 79,
        "buyDate": "2026-02-11",
        "buyPrice": 14.7,
        "sellDate": "2026-02-18",
        "targetPrice": 26.0,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 133,
        "assetType": "stock",
        "symbol": "NETCD",
        "status": "closed",
        "strategy": "BIST",
        "lots": 25,
        "buyDate": "2026-02-05",
        "buyPrice": 46.0,
        "sellDate": "2026-02-11",
        "targetPrice": 74.0,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 134,
        "assetType": "stock",
        "symbol": "AKHAN",
        "status": "closed",
        "strategy": "BIST",
        "lots": 35,
        "buyDate": "2026-02-06",
        "buyPrice": 21.5,
        "sellDate": "2026-02-11",
        "targetPrice": 31.46,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 135,
        "assetType": "stock",
        "symbol": "THYAO",
        "status": "closed",
        "strategy": "BIST",
        "lots": 19,
        "buyDate": "2025-11-13",
        "buyPrice": 271.25,
        "sellDate": "2026-01-07",
        "targetPrice": 294.5,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 136,
        "assetType": "stock",
        "symbol": "PETKM",
        "status": "closed",
        "strategy": "BIST",
        "lots": 150,
        "buyDate": "2025-11-19",
        "buyPrice": 17.05,
        "sellDate": "2026-01-26",
        "targetPrice": 17.82,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 137,
        "assetType": "stock",
        "symbol": "PETKM",
        "status": "closed",
        "strategy": "BIST",
        "lots": 290,
        "buyDate": "2025-10-31",
        "buyPrice": 17.13,
        "sellDate": "2025-12-01",
        "targetPrice": 17.18,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 138,
        "assetType": "stock",
        "symbol": "GMSTRF",
        "status": "closed",
        "strategy": "Silver",
        "lots": 21,
        "buyDate": "2026-01-26",
        "buyPrice": 939.0,
        "sellDate": "2026-01-26",
        "targetPrice": 948.75,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 139,
        "assetType": "stock",
        "symbol": "GMSTRF",
        "status": "closed",
        "strategy": "Silver",
        "lots": 3,
        "buyDate": "2026-01-26",
        "buyPrice": 941.0,
        "sellDate": "2026-01-26",
        "targetPrice": 948.75,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 140,
        "assetType": "stock",
        "symbol": "FROTO",
        "status": "closed",
        "strategy": "BIST",
        "lots": 103,
        "buyDate": "2025-12-17",
        "buyPrice": 97.35,
        "sellDate": "2026-01-16",
        "targetPrice": 104.3,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 141,
        "assetType": "stock",
        "symbol": "FROTO",
        "status": "closed",
        "strategy": "BIST",
        "lots": 103,
        "buyDate": "2025-10-07",
        "buyPrice": 97.55,
        "sellDate": "2025-12-16",
        "targetPrice": 98.35,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 142,
        "assetType": "stock",
        "symbol": "FROTO",
        "status": "closed",
        "strategy": "BIST",
        "lots": 8,
        "buyDate": "2025-10-10",
        "buyPrice": 93.75,
        "sellDate": "2025-12-16",
        "targetPrice": 98.35,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 143,
        "assetType": "stock",
        "symbol": "FROTO",
        "status": "closed",
        "strategy": "BIST",
        "lots": 55,
        "buyDate": "2025-10-15",
        "buyPrice": 91.9,
        "sellDate": "2025-12-16",
        "targetPrice": 98.35,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 144,
        "assetType": "stock",
        "symbol": "EREGL",
        "status": "closed",
        "strategy": "BIST",
        "lots": 250,
        "buyDate": "2025-11-18",
        "buyPrice": 24.08,
        "sellDate": "2025-12-01",
        "targetPrice": 24.08,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 145,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "closed",
        "strategy": "Altın",
        "lots": 75,
        "buyDate": "2025-10-28",
        "buyPrice": 66.15,
        "sellDate": "2026-01-07",
        "targetPrice": 115.02,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 146,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "closed",
        "strategy": "Altın",
        "lots": 14,
        "buyDate": "2025-10-22",
        "buyPrice": 68.99,
        "sellDate": "2026-01-07",
        "targetPrice": 115.02,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 147,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "closed",
        "strategy": "Altın",
        "lots": 75,
        "buyDate": "2025-12-18",
        "buyPrice": 86.9,
        "sellDate": "2026-01-07",
        "targetPrice": 115.02,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 148,
        "assetType": "fx",
        "symbol": "ALTIN",
        "status": "closed",
        "strategy": "Altın",
        "lots": 52,
        "buyDate": "2025-12-29",
        "buyPrice": 96.61,
        "sellDate": "2026-01-07",
        "targetPrice": 115.02,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 149,
        "assetType": "stock",
        "symbol": "AKBNK",
        "status": "closed",
        "strategy": "BIST",
        "lots": 65,
        "buyDate": "2025-10-07",
        "buyPrice": 59.0,
        "sellDate": "2025-11-24",
        "targetPrice": 62.15,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
    {
        "id": 150,
        "assetType": "stock",
        "symbol": "AKBNK",
        "status": "closed",
        "strategy": "BIST",
        "lots": 24,
        "buyDate": "2025-10-30",
        "buyPrice": 59.1,
        "sellDate": "2025-11-24",
        "targetPrice": 62.15,
        "stopLoss": None,
        "takeProfit": None,
        "affectCash": True,
    },
]


def load_data():
  if os.path.exists(DATA_FILE):
    try:
      with open(DATA_FILE, "r", encoding="utf-8") as f:
        return json.load(f)
    except:
      return DEFAULT_TRANSACTIONS
  return DEFAULT_TRANSACTIONS


def save_data(data):
  with open(DATA_FILE, "w", encoding="utf-8") as f:
    json.dump(data, f, ensure_ascii=False, indent=4)


if "transactions" not in st.session_state:
  st.session_state.transactions = load_data()


def format_curr(val, currency="TRY", usd_rate=42.50):
  if currency == "USD":
    val = val / usd_rate
    return f"${val:,.2f}"
  return f"₺{val:,.2f}"


def calc_commission(tx, comm_rate):
  if tx.get("assetType") == "fx" and tx.get("symbol") in [
      "TRY",
      "TL",
      "NAKİT",
  ]:
    return 0.0
  comm_dec = comm_rate / 1000.0
  vol = tx["lots"] * tx["buyPrice"]
  if tx["status"] == "open":
    return vol * comm_dec
  else:
    target_vol = tx["lots"] * tx["targetPrice"]
    return (vol + target_vol) * comm_dec


# --- KENAR ÇUBUĞU ---
with st.sidebar:
  st.title("⚙️ Kontrol Paneli")
  currency = st.radio("Para Birimi", ["TRY", "USD"], horizontal=True)
  usd_rate = st.number_input("USD/TRY Kuru", value=42.50, step=0.1)
  comm_rate = st.number_input("Komisyon Oranı (Binde)", value=0.0, step=0.1)

  st.divider()
  st.subheader("🔍 Filtreleme")
  strategies = ["Tümü"] + list(
      set(
          [
              t.get("strategy", "Belirtilmedi")
              for t in st.session_state.transactions
              if t.get("strategy")
          ]
      )
  )
  selected_strategy = st.selectbox("Strateji Filtresi", strategies)

  st.divider()
  if st.button("Verileri Sıfırla / Varsayılanı Yükle"):
    st.session_state.transactions = DEFAULT_TRANSACTIONS
    save_data(DEFAULT_TRANSACTIONS)
    st.success("Varsayılan veriler yüklendi!")
    st.rerun()

# --- ANA EKRAN ---
st.title("📈 Portföy & Kasa Yönetimi Terminali")
st.markdown("Nakit akışı, hisse takibi ve ileri düzey risk metrikleri.")

filtered_txs = st.session_state.transactions
if selected_strategy != "Tümü":
  filtered_txs = [
      t for t in filtered_txs if t.get("strategy") == selected_strategy
  ]

cash_balance = 0.0
active_investments = 0.0
total_pnl = 0.0
open_total_cost = 0.0
open_total_current_val = 0.0

gross_prof = 0.0
gross_loss = 0.0
win_count = 0
loss_count = 0

running_realized_equity = 0.0
peak_equity = 0.0
max_dd = 0.0

closed_sorted = sorted(
    [t for t in filtered_txs if t["status"] == "closed"],
    key=lambda x: x.get("sellDate", ""),
)

for tx in filtered_txs:
  vol = tx["lots"] * tx["buyPrice"]
  cur = tx["lots"] * tx["targetPrice"]
  is_try = tx.get("assetType") == "fx" and tx.get("symbol") in [
      "TRY",
      "TL",
      "NAKİT",
  ]
  comm = 0.0 if is_try else calc_commission(tx, comm_rate)
  affect_cash = tx.get("affectCash", True)

  if is_try:
    if affect_cash:
      if tx["status"] == "open":
        cash_balance += tx["lots"]
      elif tx["status"] == "closed":
        cash_balance -= tx["lots"]
  else:
    if tx["status"] == "open":
      buy_comm = vol * (comm_rate / 1000.0)
      if affect_cash:
        cash_balance -= vol + buy_comm
      active_investments += cur

      open_total_cost += vol
      open_total_current_val += cur

      net_pr = (cur - vol) - buy_comm
      total_pnl += net_pr
    elif tx["status"] == "closed":
      buy_comm = vol * (comm_rate / 1000.0)
      sell_comm = cur * (comm_rate / 1000.0)
      if affect_cash:
        cash_balance -= vol + buy_comm
        cash_balance += cur - sell_comm
      net_pr = (cur - vol) - (buy_comm + sell_comm)
      total_pnl += net_pr
      if net_pr > 0:
        gross_prof += net_pr
        win_count += 1
      elif net_pr < 0:
        gross_loss += abs(net_pr)
        loss_count += 1

for tx in closed_sorted:
  if tx.get("assetType") == "fx" and tx.get("symbol") in [
      "TRY",
      "TL",
      "NAKİT",
  ]:
    continue
  vol = tx["lots"] * tx["buyPrice"]
  cur = tx["lots"] * tx["targetPrice"]
  np = (cur - vol) - calc_commission(tx, comm_rate)
  running_realized_equity += np
  if running_realized_equity > peak_equity:
    peak_equity = running_realized_equity
  dd = peak_equity - running_realized_equity
  if dd > max_dd:
    max_dd = dd

total_net_worth = cash_balance + active_investments
total_trades = win_count + loss_count
win_rate = (win_count / total_trades * 100) if total_trades > 0 else 0.0
profit_factor = (
    (gross_prof / gross_loss)
    if gross_loss > 0
    else (float("inf") if gross_prof > 0 else 0.0)
)
avg_win = gross_prof / win_count if win_count > 0 else 0.0
avg_loss = gross_loss / loss_count if loss_count > 0 else 0.0
risk_reward = (
    (avg_win / avg_loss)
    if avg_loss > 0
    else (float("inf") if avg_win > 0 else 0.0)
)

open_unrealized_pnl = open_total_current_val - open_total_cost
open_unrealized_pct = (
    (open_unrealized_pnl / open_total_cost * 100)
    if open_total_cost > 0
    else 0.0
)

# --- 1. SATIR METRİKLERİ ---
col1, col2, col3, col4 = st.columns(4)
col1.markdown(
    f"""<div class="glass-card" style="border-left: 4px solid #34d399;">
    <p style="font-size:11px; color:#94a3b8; text-transform:uppercase;">Nakit
    Para (Kasa)</p><h3
    style="color:#34d399;">{format_curr(cash_balance, currency,
    usd_rate)}</h3></div>""",
    unsafe_allow_html=True,
)
col2.markdown(
    f"""<div class="glass-card" style="border-left: 4px solid #60a5fa;">
    <p style="font-size:11px; color:#94a3b8; text-transform:uppercase;">Aktif
    Yatırımlar</p><h3
    style="color:#60a5fa;">{format_curr(active_investments, currency,
    usd_rate)}</h3></div>""",
    unsafe_allow_html=True,
)
col3.markdown(
    f"""<div class="glass-card" style="border-left: 4px solid #818cf8;">
    <p style="font-size:11px; color:#94a3b8; text-transform:uppercase;">Aktif
    Toplam Varlık</p><h3>{format_curr(total_net_worth, currency,
    usd_rate)}</h3></div>""",
    unsafe_allow_html=True,
)

pnl_color = "#34d399" if total_pnl >= 0 else "#f87171"
open_pnl_color = "#34d399" if open_unrealized_pnl >= 0 else "#f87171"
col4.markdown(
    f"""<div class="glass-card" style="border-left: 4px solid {pnl_color};">
    <p style="font-size:11px; color:#94a3b8; text-transform:uppercase;">Toplam
    Net Kâr</p><h3 style="color:{pnl_color};">{format_curr(total_pnl, currency,
    usd_rate)}</h3>
    <p style="font-size:10px; color:#94a3b8; margin-top:4px;">Açık Poz. K/Z: <span
    style="color:{open_pnl_color};">{format_curr(open_unrealized_pnl, currency,
    usd_rate)} (%{open_unrealized_pct:.2f})</span></p></div>""",
    unsafe_allow_html=True,
)

# --- 2. SATIR RİSK METRİKLERİ ---
r1, r2, r3, r4 = st.columns(4)
r1.metric("Kazanma Oranı (Win Rate)", f"%{win_rate:.1f}")
r2.metric(
    "Kâr Faktörü (Profit Factor)",
    f"{profit_factor:.2f}" if profit_factor != float("inf") else "∞",
)
r3.metric(
    "Risk / Ödül (R/R)",
    f"{risk_reward:.2f}" if risk_reward != float("inf") else "∞",
)
r4.metric("Max Düşüş (MDD)", format_curr(max_dd, currency, usd_rate))

st.divider()

# --- GRAFİK GÖRSELLEŞTİRME BÖLÜMÜ (Plotly ile Yüzde Etiketli Daire Grafiği) ---
st.subheader("📊 Portföy Görselleştirme & Grafik Analizi")
chart_category = st.selectbox(
    "Grafik Kategorisi",
    ["Açık Pozisyonlar Varlık Dağılımı", "Açık Pozisyonlar Kâr / Zarar Dağılımı"],
)
chart_type = st.selectbox(
    "Grafik Türü", ["Daire / Pasta", "Sütun Grafiği", "Çizgi Grafiği"]
)

open_df_all = pd.DataFrame(
    [t for t in filtered_txs if t.get("status") == "open"]
)
if not open_df_all.empty:
  chart_data_list = []
  for sym, group in open_df_all.groupby("symbol"):
    total_lots = group["lots"].sum()
    total_cost = (group["lots"] * group["buyPrice"]).sum()
    cur_price = group["targetPrice"].iloc[0]
    cur_val = total_lots * cur_price
    pnl = cur_val - total_cost

    chart_data_list.append(
        {"Sembol": sym, "Değer": cur_val, "Kâr_Zarar": pnl}
    )

  chart_df = pd.DataFrame(chart_data_list)

  if chart_category == "Açık Pozisyonlar Varlık Dağılımı":
    val_col = "Değer"
  else:
    val_col = "Kâr_Zarar"

  if chart_type == "Daire / Pasta":
    fig = px.pie(
        chart_df,
        names="Sembol",
        values=val_col,
        hole=0.4,
        title=f"{chart_category} ({chart_type})",
    )
    fig.update_traces(textinfo="percent+label", textfont_size=12)
    st.plotly_chart(fig, use_container_width=True)
  elif chart_type == "Sütun Grafiği":
    fig = px.bar(
        chart_df,
        x="Sembol",
        y=val_col,
        color="Sembol",
        title=f"{chart_category} ({chart_type})",
    )
    st.plotly_chart(fig, use_container_width=True)
  else:
    fig = px.line(
        chart_df,
        x="Sembol",
        y=val_col,
        markers=True,
        title=f"{chart_category} ({chart_type})",
    )
    st.plotly_chart(fig, use_container_width=True)
else:
  st.info("Grafik oluşturulacak açık pozisyon bulunmuyor.")

st.divider()

# --- ANA KATEGORİ SEKMELERİ ---
st.subheader("📋 Detaylı Varlık Defteri & Konsolide Özetler")
tab_borsa, tab_ipo, tab_nakit, tab_tum = st.tabs(
    ["📊 Borsa", "🚀 Halka Arz", "💵 Nakit / Döviz / Altın", "📂 Tüm İşlemler"]
)


def render_category_views(subset_type, category_name):
  sub_open, sub_closed = st.tabs(["🟢 Açık Pozisyonlar", "🔴 Kapalı İşlemler"])

  with sub_open:
    df_open = pd.DataFrame(
        [
            t
            for t in st.session_state.transactions
            if t.get("assetType") == subset_type and t.get("status") == "open"
        ]
    )
    if not df_open.empty:
      view_mode = st.radio(
          "Görünüm Modu",
          ["Detaylı Liste (Düzenlenebilir)", "Varlık Bazlı Toplu Özet"],
          key=f"mode_open_{subset_type}",
          horizontal=True,
      )

      if view_mode == "Detaylı Liste (Düzenlenebilir)":
        edited_open = st.data_editor(
            df_open,
            key=f"editor_{subset_type}_open",
            use_container_width=True,
            num_rows="dynamic",
        )
        if st.button(
            f"💾 {category_name} - Açık Değişiklikleri Kaydet",
            key=f"btn_{subset_type}_open",
        ):
          open_ids = {t["id"] for t in df_open.to_dict(orient="records")}
          other_txs = [
              t
              for t in st.session_state.transactions
              if t["id"] not in open_ids
              or t.get("assetType") != subset_type
              or t.get("status") != "open"
          ]
          st.session_state.transactions = other_txs + edited_open.to_dict(
              orient="records"
          )
          save_data(st.session_state.transactions)
          st.success("Açık pozisyon değişiklikleri kaydedildi!")
          st.rerun()
      else:
        summary_list = []
        for sym, group in df_open.groupby("symbol"):
          total_lots = group["lots"].sum()
          total_cost_vol = (group["lots"] * group["buyPrice"]).sum()
          avg_cost = total_cost_vol / total_lots if total_lots > 0 else 0
          current_price = group["targetPrice"].iloc[0]
          current_val = total_lots * current_price

          comm_val = sum([
              calc_commission(row, comm_rate) for _, row in group.iterrows()
          ])
          net_pnl = (current_val - total_cost_vol) - comm_val
          pnl_pct = (
              (net_pnl / total_cost_vol * 100) if total_cost_vol > 0 else 0
          )

          summary_list.append({
              "Varlık / Sembol": sym,
              "Toplam Lot": total_lots,
              "Ortalama Maliyet": round(avg_cost, 2),
              "Güncel Fiyat": current_price,
              "Güncel Değer": round(current_val, 2),
              "Güncel Kâr/Zarar (₺)": round(net_pnl, 2),
              "Güncel Kâr/Zarar (%)": f"%{pnl_pct:.2f}",
          })
        summary_df = pd.DataFrame(summary_list)
        st.dataframe(summary_df, use_container_width=True)
    else:
      st.info(f"{category_name} kategorisinde açık pozisyon bulunmuyor.")

  with sub_closed:
    df_closed = pd.DataFrame(
        [
            t
            for t in st.session_state.transactions
            if t.get("assetType") == subset_type and t.get("status") == "closed"
        ]
    )
    if not df_closed.empty:
      view_mode_c = st.radio(
          "Görünüm Modu",
          ["Detaylı Liste (Düzenlenebilir)", "Varlık Bazlı Toplu Özet"],
          key=f"mode_closed_{subset_type}",
          horizontal=True,
      )

      if view_mode_c == "Detaylı Liste (Düzenlenebilir)":
        edited_closed = st.data_editor(
            df_closed,
            key=f"editor_{subset_type}_closed",
            use_container_width=True,
            num_rows="dynamic",
        )
        if st.button(
            f"💾 {category_name} - Kapalı Değişiklikleri Kaydet",
            key=f"btn_{subset_type}_closed",
        ):
          closed_ids = {t["id"] for t in df_closed.to_dict(orient="records")}
          other_txs = [
              t
              for t in st.session_state.transactions
              if t["id"] not in closed_ids
              or t.get("assetType") != subset_type
              or t.get("status") != "closed"
          ]
          st.session_state.transactions = other_txs + edited_closed.to_dict(
              orient="records"
          )
          save_data(st.session_state.transactions)
          st.success("Kapalı işlem değişiklikleri kaydedildi!")
          st.rerun()
      else:
        summary_list_c = []
        for sym, group in df_closed.groupby("symbol"):
          total_lots = group["lots"].sum()
          total_buy_vol = (group["lots"] * group["buyPrice"]).sum()
          total_sell_vol = (group["lots"] * group["targetPrice"]).sum()
          avg_buy = total_buy_vol / total_lots if total_lots > 0 else 0
          avg_sell = total_sell_vol / total_lots if total_lots > 0 else 0

          total_comm = sum([
              calc_commission(row, comm_rate) for _, row in group.iterrows()
          ])
          net_pnl = (total_sell_vol - total_buy_vol) - total_comm
          pnl_pct = (
              (net_pnl / total_buy_vol * 100) if total_buy_vol > 0 else 0
          )

          summary_list_c.append({
              "Varlık / Sembol": sym,
              "Toplam İşlem Adedi": len(group),
              "Toplam Lot": total_lots,
              "Ort. Alış Maliyeti": round(avg_buy, 2),
              "Ort. Satış Fiyatı": round(avg_sell, 2),
              "Toplam Net Kâr/Zarar (₺)": round(net_pnl, 2),
              "Getiri (%)": f"%{pnl_pct:.2f}",
          })
        summary_df_c = pd.DataFrame(summary_list_c)
        st.dataframe(summary_df_c, use_container_width=True)
    else:
      st.info(f"{category_name} kategorisinde kapalı işlem bulunmuyor.")


with tab_borsa:
  render_category_views("stock", "Borsa")

with tab_ipo:
  render_category_views("ipo", "HalkaArz")

with tab_nakit:
  render_category_views("fx", "NakitDovizAltin")

with tab_tum:
  all_df = pd.DataFrame(st.session_state.transactions)
  if not all_df.empty:
    edited_all = st.data_editor(
        all_df, key="editor_all", use_container_width=True, num_rows="dynamic"
    )
    if st.button("💾 Tüm Listeyi Güncelle ve Kaydet", key="btn_all"):
      st.session_state.transactions = edited_all.to_dict(orient="records")
      save_data(st.session_state.transactions)
      st.success("Tüm liste başarıyla güncellendi!")
      st.rerun()