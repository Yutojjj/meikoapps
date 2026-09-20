# Vercel 公開前の設定

1. GitHub Desktop でこのフォルダを GitHub に push します。
2. Vercel で GitHub リポジトリを Import し、Framework Preset は `Other`、Build Command は空欄、Output Directory は `.` にして Deploy します。
3. Firebase Console の Authentication で、本番用のログイン方式を有効にします。現在の画面内の ID／パスワード入力はデモ用で、Firebase Authentication に接続した本番ログインではありません。
4. Firestore Database のルールを、少なくとも未ログインの読み書きを拒否する設定に変更します。Firebase の `apiKey` と設定値はブラウザに置かれる公開設定であり、秘密情報ではありません。データを守るのは Firestore Rules と Firebase Authentication です。

公開用の最小ルール例です。実運用では、管理者・講師・生徒の権限ごとにさらに分けてください。

```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

Vercel は HTTPS を自動で提供するため、公開後にスマホ・PCのブラウザメニューから「インストール」または「ホーム画面に追加」を選ぶと PWA として利用できます。
