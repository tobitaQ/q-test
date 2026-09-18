# 再現テストのお願い（5〜10 分・費用なし・専門知識不要）

## これは何か

論文「Measuring Metric and Sampling Distortions in Penalty-Encoded QAOA Portfolio Benchmarks」（IEEE TQE 投稿中）の
公開パッケージが、**著者と無関係の環境で README どおりに動くか**を確かめるテストです。
量子計算・GPU・クラウドは使いません。あなたの環境で「コミット済みの結果ファイルから論文の図と表を作り直し、
原稿の全表 2,228 セルと一致するか」を機械的に確認するだけです（これを「水準 1 の再現」と呼んでいます）。

**動かなくても構いません。** 止まった場所とエラーが分かれば、それが README の不備の発見として同じ価値を持ちます。

人を対象にした実験ではありません。個人情報は取りません。報告に入るのは OS 名・Python・ライブラリのバージョンと、
テスト・図・表の結果、所要時間だけです（ホスト名・ユーザ名・パスは含めない作りにしてあります。送る前に `report.txt` を見て、
気になるものがあれば消してください）。論文には論文には「Windows 11／Python 3.12 の環境で 7 分で完了」のように匿名で載ります。

## やり方 A：ブラウザだけ（Google アカウントが要る・おすすめ）

1. 開く → **[Colab で開く](https://colab.research.google.com/github/tobitaQ/qaoa-benchmark-reproduction/blob/main/notebooks/reproduce_level1.ipynb)**
2. 上から順にセル左の ▶ を押す（4 つ。1 つ目が 2〜3 分、あとは 1〜2 分ずつ）
3. **最後のセルの出力を全部コピーして送る**（`report.txt` の中身が表示されます）

## やり方 B：自分の PC（Windows／Mac／Linux のどれか。OS が違うほど価値があります）

用意：Python 3.11 以上・git。Windows は WSL か Git Bash 推奨（PowerShell でも 1 行ずつなら可）。

```bash
git clone https://github.com/tobitaQ/qaoa-portfolio-penalty-benchmark.git
cd qaoa-portfolio-penalty-benchmark
git checkout v1.0
python -m venv .venv && . .venv/bin/activate     # Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -r requirements.txt                  # 2〜3 分
python -m pytest -q                              # 1〜2 分
python -m notebooks.generate_tqe_figures         # 約 30 秒
cd papers && python check_tables.py && cd ..     # 数秒
```

報告をまとめて作る（bash。PowerShell の人は各コマンドの最後の数行を手でコピー）：

```bash
{ uname -srm; python --version; git log --oneline -1
  pip freeze | grep -iE "^(pennylane|numpy|pandas|matplotlib|scipy)=="
  python -m pytest -q 2>&1 | tail -1
  ( cd papers && python check_tables.py 2>&1 | tail -4 )
  sha256sum notebooks/figures/*.pdf 2>/dev/null || shasum -a 256 notebooks/figures/*.pdf
} > report.txt; cat report.txt
```

## 送ってほしいもの

- `report.txt` の中身（A なら最後のセルの出力）
- 開始から終了までのおおよその時間
- 途中で止まった場合：どのコマンドで、エラーの最後の 20 行くらい
- 一言：README で分かりにくかった所（あれば）

送り先：依頼した本人（メール・チャットどちらでも）。

## よくある疑問

- **図のハッシュが違う** → Matplotlib のバージョン差で起きます。判定は `check_tables.py` の「一致セル数」で見ます。
- **pip で失敗した** → その出力だけ送ってください。環境差の記録が目的なので、それで十分です。
- **時間がない** → やり方 A の 3 手順だけで足ります。
