# パススルーメッシュ確認用プロジェクト

このプロジェクトはフォーク元のサンプルプロジェクト (Unreal-OcclusionSampe) を改造して、パススルーメッシュの動作を検証するプロジェクトです。

## Occlusionsレベルについて

<img width="1434" height="584" alt="image" src="https://github.com/user-attachments/assets/508b5c39-0602-4532-91a6-9940573b797e" />

Occlusionレベルの以下のすべてのメッシュに対してパススルーメッシュを適用 (`OculusXRPassthroughLayerCompoent::AddStaticSurfaceGeometry()`を使用)

- BP_EnvironmentDepth (Plane Mesh): ⭕️正常にソフトオクリュージョンが機能します
- BP_Cube (1M_Cube): ❌️ソフトオクリュージョンの範囲が黒いモヤのような見た目となります
- BP_Plane (SM Plane): ❌️ソフトオクリュージョンの範囲が黒いモヤのような見た目となります

## パススルーメッシュの適用について

Occusionsレベルのレベルブループリントの `Begin Play`において、`PlayerPawn(VRPawn)`にアタッチ済みの`OculusXRPassthroughLayerCompoent`を使用して`Add Static Surface Geometry`でパススルーメッシュを適用しています。

<img width="1894" height="571" alt="image" src="https://github.com/user-attachments/assets/5511d8ab-25fc-4f58-8057-6b412aff1962" />

## VRPawnの設定
VRPawnには、`OculusXRPassthroughLayer`をアタッチして、パススルーメッシュを使うようにしています。

<img width="273" height="123" alt="image" src="https://github.com/user-attachments/assets/f30840b3-3ae0-4e32-961b-888d27a88d3a" />
<img width="546" height="246" alt="image" src="https://github.com/user-attachments/assets/9fa3dfd9-5b74-4a4a-9889-bc339fbb80b4" />

- `Support Depth`: ON
- `Stereo Layer Shape`: User Defined Passthrough Layer
- `Layer Placement`: Overlay

この設定によりサンプルプロジェクトで使用していたOccusionsレベルのレベルブループリントの `Persistent Passthrough` は無効化しています。

<img width="886" height="376" alt="image" src="https://github.com/user-attachments/assets/a95c75c3-d88b-46ec-844a-d4fc2bfd1974" />
