## リポジトリについて
Stable Diffusionの学習、画像生成、その他のスクリプトを入れたリポジトリです。

GUIやPowerShellスクリプトなど、より使いやすくする機能が[bmaltais氏のリポジトリ](https://github.com/bmaltais/kohya_ss)で提供されています（英語です）のであわせてご覧ください。bmaltais氏に感謝します。

[kohya-ss氏のWindows用リポジトリ](https://github.com/kohya-ss/sd-scripts)をWSL2 on Dockerで利用できるようにしたリポジトリです。kohya-ss氏に感謝します。

以下記載はほぼオリジナルのリポジトリと同様で、環境構築部分を変更しています。


以下のスクリプトがあります。

* DreamBooth、U-NetおよびText Encoderの学習をサポート
* fine-tuning、同上
* 画像生成
* モデル変換（Stable Diffision ckpt/safetensorsとDiffusersの相互変換）

## 使用法について

当リポジトリ内およびnote.comに記事がありますのでそちらをご覧ください（将来的にはすべてこちらへ移すかもしれません）。

* note.com [環境整備とDreamBooth学習スクリプトについて](https://note.com/kohya_ss/n/nba4eceaa4594)
* [fine-tuningのガイド](./fine_tune_README_ja.md):
BLIPによるキャプショニングと、DeepDanbooruまたはWD14 taggerによるタグ付けを含みます
* note.com [画像生成スクリプト](https://note.com/kohya_ss/n/n2693183a798e)
* note.com [モデル変換スクリプト](https://note.com/kohya_ss/n/n374f316fe4ad)

## 想定環境

- Windows 10
  - RTX2080Ti
- Docker Desktop（インストールは[ここ](https://qiita.com/sumita_v09/items/810685f77cdd0586db16)を参照）

## Docker Imageの作成

Dockerが起動している状態で以下コマンドを実行する。

```shell
docker build . -t diffuser:0.1
```

## Dockerコンテナを起動・環境構築（残った分）

```shell
docker run --gpus all -it --rm -v {このフォルダまでの絶対パス}/sd-scripts-wsl2-docker:/work diffuser:0.1

(ここからコンテナ内での操作)
cd /work
accelerate config

```

accelerate configの質問には以下のように答えてください。（bf16で学習する場合、最後の質問にはbf16と答えてください。）

※0.15.0から日本語環境では選択のためにカーソルキーを押すと落ちます（……）。数字キーの0、1、2……で選択できますので、そちらを使ってください。

```txt
- This machine
- No distributed training
- NO
- NO
- NO
- all
- fp16
```

※場合によって ``ValueError: fp16 mixed precision requires a GPU`` というエラーが出ることがあるようです。この場合、6番目の質問（
``What GPU(s) (by id) should be used for training on this machine as a comma-separated list? [all]:``）に「0」と答えてください。（id `0`のGPUが使われます。）


## 謝意

LoRAの実装は[cloneofsimo氏のリポジトリ](https://github.com/cloneofsimo/lora)を基にしたものです。感謝申し上げます。

## ライセンス

スクリプトのライセンスはASL 2.0ですが（Diffusersおよびcloneofsimo氏のリポジトリ由来のものも同様）、一部他のライセンスのコードを含みます。

[Memory Efficient Attention Pytorch](https://github.com/lucidrains/memory-efficient-attention-pytorch): MIT

[bitsandbytes](https://github.com/TimDettmers/bitsandbytes): MIT

[BLIP](https://github.com/salesforce/BLIP): BSD-3-Clause


