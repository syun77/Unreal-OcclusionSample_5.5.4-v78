# XR Soft Occlusions Depth Biasの検証
## 概要
ここでは XR Soft Occlusions Depth Bias の検証を行った結果をまとめます。

## 検証内容
XR Soft Occlusions Depth Biasはソフトオクリュージョンに関するマテリアルの設定で、Oculus VR ForkのUnreal Engineのみで設定可能なマテリアルの項目です。

<img width="484" height="206" alt="image" src="https://github.com/user-attachments/assets/4879bfaa-da76-43bd-847e-6f70aaae66da" />

この項目は、仮想オブジェクトが現実の壁や床などと非常に近い位置にあると、Depth（深度）の競合＝“ちらつき”や“めり込み”が発生しますが、これを避けるためのバイアス値です。
通常は正の値で "0.06" などといった極めて小さい値を指定します。これにより、仮想オブジェクトが現実の表面よりもほんの少し優先的に描画されるようになります。

## 検証結果

XR Soft Occlusions Depth Biasを設定することによって、デプス処理の描画範囲が黒いモヤとなる問題の解決はできませんでした。

## 補足事項

今回の検証とは直接関係しませんが、VRPreviewで検証した際は、パススルーメッシュの一部に黒いモヤはあるもの、デプス処理の描画範囲は正常に描画されているようでした。
以下は VRPreview (Unreal Engineから VRPreviewで実行したときの状態) で確認したときの動画です。

![ue5](https://github.com/user-attachments/assets/350ff2b7-f20f-429b-96bf-bea916b8036f)

ですが、Meta Quest3 実機へ転送 (AnroidのAPKビルド) をした際にはデプス処理の部分は黒いモヤのままでした。

![ue5](https://github.com/user-attachments/assets/45b96f5c-b9db-4ac7-9352-d6cbf095a821)
