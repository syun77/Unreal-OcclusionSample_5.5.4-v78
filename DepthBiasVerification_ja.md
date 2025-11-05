# XR Soft Occlusions Depth Biasの検証
## 概要
ここでは XR Soft Occlusions Depth Bias の検証を行った結果をまとめます。

## 検証内容
XR Soft Occlusions Depth Biasはソフトオクリュージョンに関するマテリアルの設定で、Oculus VR ForkのUnreal Engineのみで設定可能なマテリアルの項目です。

<img width="484" height="206" alt="image" src="https://github.com/user-attachments/assets/4879bfaa-da76-43bd-847e-6f70aaae66da" />

この項目は、仮想オブジェクトが現実の壁や床などと非常に近い位置にあると、Depth（深度）の競合＝“ちらつき”や“めり込み”が発生しますが、これを避けるためのバイアス値です。
