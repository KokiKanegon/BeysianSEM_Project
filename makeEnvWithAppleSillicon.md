# **ステップ1: Anaconda仮想環境の作成**
1. **新しい仮想環境を作成**:
   ```bash
   conda create -n pystan_env python=3.10
   ```
   - `pystan_env`は仮想環境名です。必要に応じて変更してください。
   - Pythonのバージョンは3.8または3.9が推奨されます。

2. **仮想環境を有効化**:
   ```bash
   conda activate pystan_env
   ```

---

# **ステップ2: 必要な依存関係をインストール**
1. **コンパイラと基本的なパッケージをインストール**:
   ```bash
   conda install numpy cython gcc_linux-64 gxx_linux-64
   ```
   - macOSの場合、`gcc_linux-64`の代わりに`clang`が使われます（Anacondaが自動で選択する場合もあります）。

2. **最新の`pip`をアップグレード**:
   ```bash
   python -m pip install --upgrade pip setuptools wheel
   ```

---

# **ステップ3: httpstanをソースからインストール**
上記の手順（`httpstan`のソースビルド）をAnaconda仮想環境内で実行します。

1. **httpstanのソースコードをダウンロード**:
    - ダウンロードは下のサイトで行った。
    - [httpstan GitHubリリース](https://github.com/stan-dev/httpstan/releases/tag/4.13.0)から最新バージョンの`.zip`または`.tar.gz`をダウンロードし、解凍します。

2. **ソースコードのビルド**:
   - ターミナルで解凍したフォルダに移動し、以下を実行します:
     ```bash
     make
     python -m pip install poetry
     python -m poetry build
     python -m pip install dist/*.whl
     ```

---

# **ステップ4: PyStanをインストール**
- 以下を実行してPyStanをインストールします:
  ```bash
  pip install pystan --no-binary pystan
  ```

---

# **ステップ5: 動作確認**
以下のテストコードを実行して、PyStanが正しく動作することを確認します:

```python
from stan import StanModel

model_code = """
parameters {
    real y;
}
model {
    y ~ normal(0, 1);
}
"""
sm = StanModel(model_code=model_code)
fit = sm.sample(num_chains=4, num_samples=1000)
print(fit)
```

---

### **注意点**
- **Anacondaの特性**:
  - 仮想環境のPythonバージョンとパッケージバージョンの互換性を常に確認してください。
  - Anaconda環境に不要なパッケージが多くなるのを避けるために、プロジェクトごとに専用環境を作ることをお勧めします。

- **Apple Silicon特有の設定**:
  - AnacondaのPythonがIntel向けバージョンの場合、M1/M2向けに最適化された`miniforge`を利用するのも良い選択肢です。

不明点があれば教えてください！