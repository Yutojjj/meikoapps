# Vercel 公開前の設定

1. GitHub Desktop でこのフォルダを GitHub に push します。
2. Vercel で GitHub リポジトリを Import し、Framework Preset は `Other`、Build Command は空欄、Output Directory は `.` にして Deploy します。
3. 現在のログインは仮実装です。ログイン画面で ID `admin`、パスワード `admin` を入力します。本番用のFirebase Authenticationは後で切り替えます。
4. Firebaseのウェブアプリ設定値は `juku-admin-home.html` に設定済みです。ログイン後にFirestoreからデータを読み込み、各保存操作でFirestoreへ保存します。
5. 仮運用用の全許可ルールは、同梱の `firestore.rules` にあります。本番公開前には必ず認証必須のルールへ変更してください。

公開用の最小ルール例です。実運用では、管理者・講師・生徒の権限ごとにさらに分けてください。

```text
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

Vercel は HTTPS を自動で提供するため、公開後にスマホ・PCのブラウザメニューから「インストール」または「ホーム画面に追加」を選ぶと PWA として利用できます。
