# パススルーメッシュ確認用プロジェクト

このプロジェクトはフォーク元のサンプルプロジェクト (Unreal-OcclusionSampe) を改造して、パススルーメッシュの動作を検証するプロジェクトです。

>このプロジェクトは Meta Developer Centerに検証用として提出するプロジェクト・Readmeとなります。

## 概要

- ソフトオクリュージョンを有効にすると、パススルーを適用したメッシュについて、正常にパススルーが表示されるメッシュもあるが、その一方パススルーがまったく表示されなくなるメッシュとなるものも存在します
- ソフトオクリュージョンによりデプス処理が正常に行われるメッシュ (`Plane` のみ)が存在する一方、それ以外のパススルーメッシュではデプスが適用される部分が黒いモヤの状態となってしまう

上記2点の問題がなぜ発生するのか、またこれらの問題を解消するにはメッシュの設定やその他の設定が何が必要となるのかをご教授いただきたいです。

またソフトオクリュージョンに下記の制約があるかどうか気になっているため合わせてご回答いただけると助かります。

- Depth APIを使用した場合に、遠景メッシュ（遠く離れた部分にある背景となるメッシュ）を正常に表示する方法はあるのか。（ハードオクリュージョンでも遠景メッシュが正しく表示できないことを確認しています）

## サンプルプロジェクトの説明

以下にこのサンプルプロジェクトを元のサンプルプロジェクトからどのように変更したのかを記載します。

### Occlusionsレベルについて

Occlusionsレベルについて、パススルーメッシュを適用した状態でのソフトオクリュージョンを検証するため以下のようにメッシュを配置しました。

<img width="1434" height="584" alt="image" src="https://github.com/user-attachments/assets/508b5c39-0602-4532-91a6-9940573b797e" />

Occlusionレベルの以下のすべてのメッシュに対してパススルーメッシュを適用 (`OculusXRPassthroughLayerCompoent::AddStaticSurfaceGeometry()`を使用)

- Plane (BP_EnvironmentDepth): ⭕️正常にソフトオクリュージョンが機能します
- 1M_Cube (BP_Cube): ❌️ソフトオクリュージョンの範囲が黒いモヤのような見た目となります。場所によってはパススルーが表示されず大部分が黒い状態になることもあります
- SM_Plane (BP_Plane): ❌️ソフトオクリュージョンの範囲が黒いモヤのような見た目となります。場所によってはパススルーが表示されず大部分が黒い状態になることもあります

<img width="434" height="554" alt="image" src="https://github.com/user-attachments/assets/9cd7b198-264b-4c44-8e06-e973896680e2" />

### パススルーメッシュの適用について

Occusionsレベルのレベルブループリントの `Begin Play`において、`PlayerPawn(VRPawn)`にアタッチ済みの`OculusXRPassthroughLayerCompoent`を使用して`Add Static Surface Geometry`でパススルーメッシュを適用しています。

<img width="1894" height="571" alt="image" src="https://github.com/user-attachments/assets/5511d8ab-25fc-4f58-8057-6b412aff1962" />

### VRPawnの設定
VRPawnには、`OculusXRPassthroughLayer`をアタッチして、パススルーメッシュを使うようにしています。

<img width="273" height="123" alt="image" src="https://github.com/user-attachments/assets/f30840b3-3ae0-4e32-961b-888d27a88d3a" />
<img width="546" height="246" alt="image" src="https://github.com/user-attachments/assets/9fa3dfd9-5b74-4a4a-9889-bc339fbb80b4" />

- `Support Depth`: ON
- `Stereo Layer Shape`: User Defined Passthrough Layer
- `Layer Placement`: Overlay

この設定によりサンプルプロジェクトで使用していたOccusionsレベルのレベルブループリントの `Persistent Passthrough` は無効化しています。

<img width="886" height="376" alt="image" src="https://github.com/user-attachments/assets/a95c75c3-d88b-46ec-844a-d4fc2bfd1974" />

### メッシュの設定について

`1M_Cube` と `SM_Plane` について、パススルーを適用するため `Allow CPUAccess` を ON にしています。

<img width="341" height="62" alt="image" src="https://github.com/user-attachments/assets/561b52f9-6c48-466b-afb7-e188e0eaf5a3" />

### ログファイル

実行ログは以下のファイルとなります。

https://github.com/syun77/Unreal-OcclusionSample_5.5.4-v78/blob/main/Log_20251104.log

## 動画
最後にサンプルプロジェクトの実行結果となるGIF動画を添付しておきます。 

### ハードオクリュージョン
ハードオクリュージョンについての動作については問題ありません。デプスによって前後関係が正常に処理されます。

![ue5](https://github.com/user-attachments/assets/fead03a4-0e3e-4741-b16e-fc77fa21017f)

### ソフトオクリュージョン
ソフトオクリュージョンはいくつかの奇妙な表示がされます。

![ue5](https://github.com/user-attachments/assets/8077446d-b6b4-4868-8312-7105641a1feb)

- パススルーが正常に表示されるメッシュと、それとは別に大部分が黒くなる（パススルーが表示されない）メッシュが存在する (ハードオクリュージョンの設定の場合は正常にパススルーメッシュとして機能しています)
- `Plane` (BP_EnvironmentDepth) 以外のメッシュでは、ソフトオクリュージョンのデプス処理によって見た目が黒くなってしまう
