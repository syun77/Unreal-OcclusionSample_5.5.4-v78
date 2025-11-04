# パススルーメッシュ確認用プロジェクト

このプロジェクトはフォーク元のサンプルプロジェクト (Unreal-OcclusionSampe) を改造して、パススルーメッシュの動作を検証するプロジェクトです。

## Occlusionsレベルについて

<img width="1434" height="584" alt="image" src="https://github.com/user-attachments/assets/508b5c39-0602-4532-91a6-9940573b797e" />

Occlusionレベルの以下のすべてのメッシュに対してパススルーメッシュを適用 (OculusXRPassthroughLayerCompoent::AddStaticSurfaceGeometry()を使用)

- BP_EnvironmentDepth (Plane Mesh): ⭕️正常にソフトオクリュージョンが機能します
- BP_Cube (1M_Cube): ❌️ソフトオクリュージョンの範囲が黒いモヤのような見た目となります
- BP_Plane (SM Plane): ❌️ソフトオクリュージョンの範囲が黒いモヤのような見た目となります

## パススルーメッシュの適用について

Occusionsレベルのレベルブループリントの Begin Playにおいて、PlayerPawnにアタッチ済みのOculusXRPassthroughLayerCompoentを使用してAdd Static Surface Geometry でパススルーメッシュを適用しています。

<img width="1894" height="571" alt="image" src="https://github.com/user-attachments/assets/5511d8ab-25fc-4f58-8057-6b412aff1962" />
