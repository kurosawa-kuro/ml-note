# ML Note

## Python環境セットアップ

### 現在のPythonバージョン
- Python 3.12.3

### 1. pipxをインストール
```bash
sudo apt install pipx
```

### 2. Jupyter Labをインストール
```bash
pipx install jupyterlab
```

pip install -U ipykernel

### 3. 必要なライブラリをJupyter Lab環境にインストール
```bash
# Jupyter Lab環境に直接ライブラリをインストール
/home/wsl/.local/share/pipx/venvs/jupyterlab/bin/python3 -m pip install \
  duckdb pandas matplotlib seaborn numpy ipykernel
```

### 4. Jupyter Labを起動
```bash
jupyter-lab
```

## プロジェクト構成

```
ml-note/
├── README.md                    # このファイル
├── CLAUDE.md                    # Claude Code設定
├── kaggle_datasets.duckdb       # データベースファイル (274MB)
└── eda.ipynb                    # 探索的データ分析ノートブック
```

## データセット

### CMI BFRB Detection Dataset
- **データソース**: Kaggle Competition "CMI - Detect Behavior with Sensor Data"
- **データサイズ**: 574,945行 (学習), 81参加者, 8,151シーケンス
- **センサー**: IMU (加速度+回転) + ToF距離 + Thermopile温度
- **タスク**: Body-Focused Repetitive Behaviors (BFRB) 検出・分類

### データベーステーブル
- `train`: センサーデータ + ラベル (575K行)
- `test`: センサーデータのみ (107行)  
- `train_demographics`: 参加者情報 (81行)
- `test_demographics`: 参加者情報 (2行)

## 使用方法

1. 環境セットアップを実行
2. `jupyter-lab`でサーバー起動
3. `eda.ipynb`を開いてEDAを実行

## EDA結果サマリー

- **データ品質**: 加速度センサーは欠損0%、ToF_5/thm_5は5%以上欠損
- **クラス分布**: 18種類のジェスチャー、Binary分類は60/40で比較的バランス良好  
- **CV戦略**: 参加者別GroupKFoldが必須（データリーク防止）
- **目標スコア**: 0.5×(Binary F1 + Macro F1) で0.60+を目指す