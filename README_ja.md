# パススルーメッシュ確認用プロジェクト

## Occlusionsレベルについて

<img width="1424" height="584" alt="image" src="https://github.com/user-attachments/assets/8503e0da-472c-449a-8f94-2e724d6b945d" />

Occlusionレベルの以下のすべてのメッシュに対してパススルーメッシュを適用 (OculusXRPassthroughLayerCompoent::AddStaticSurfaceGeometry()を使用)

- BP_EnvironmentDepth (Plane Mesh): 正常にソフトオクリュージョンが機能します
- BP_Cube (1M_Cube): ソフトオクリュージョンの範囲が黒いモヤのような見た目となります
- BP_Plane (SM Plane): ソフトオクリュージョンの範囲が黒いモヤのような見た目となります

