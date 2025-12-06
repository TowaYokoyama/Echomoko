# Echomoko
技育博vol4出場作品

### 開発開始日：2025/11/07

音声メモをAIで賢く管理するフルスタックiOSアプリケーション

![iOS](https://img.shields.io/badge/iOS-16.0%2B-blue)
![Swift](https://img.shields.io/badge/Swift-5.9-orange)
![Node.js](https://img.shields.io/badge/Node.js-20.x-green)
![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue)

--

## 🎯 作品概要

**音声メモ × AI × ナレッジグラフ × キャラ育成** での使い方がある！


### コア機能フロー

```
🎤 音声録音 → 📝 AI自動整理 → 🕸️ ナレッジグラフ → 📤 簡単共有
```

---

## ✨ 主要機能

### 📝 メモ管理
- **音声メモ**: Whisper APIによる高精度文字起こし
- **テキストメモ**: 音声なしのシンプルなメモ作成
- **自動タグ生成**: GPT-4によるコンテキスト理解
- **ピン留め**: 重要なメモを先頭に固定
- **ドラッグ&ドロップ**: カスタム並び替え

### 🕸️ ナレッジグラフ
- **自動関連付け**: Embedding + タグベースの複合類似度計算
- **インタラクティブ可視化**: ピンチズーム・パン・ノード選択
- **サーバーサイド計算**: グラフデータをAPIで取得
- **クラスター分析**: Union-Findによるグループ検出

### 📂 カテゴリー管理
- **カスタムカテゴリー**: アイコン・カラー選択可能
- **タグベース分類**: メモのタグでカテゴリー分け
- **統計表示**: カテゴリーごとのメモ数

### 🔐 認証・セキュリティ
- **JWT認証**: アクセストークン + リフレッシュトークン
- **Apple Sign In**: ネイティブ認証対応
- **自動トークンリフレッシュ**: 401エラー時の透過的な再認証
- **Keychain**: 安全なトークン保存

---

## 🏗️ 技術スタック

### Frontend (iOS)
```
Swift 5.9+ / SwiftUI
├── アーキテクチャ: MVVM
├── 非同期処理: async/await, Combine
├── ネットワーク: URLSession
├── 音声処理: AVFoundation
├── セキュリティ: Keychain Services
├── キャッシュ: UserDefaults, メモリキャッシュ
└── 対応OS: iOS 16.0+
```

### Backend (API Server)
```
Node.js 20.x / TypeScript 5.x
├── フレームワーク: Express.js
├── データベース: MongoDB Atlas
├── キャッシュ: Redis
├── 認証: JWT + Refresh Token
├── バリデーション: Zod
├── AI統合: OpenAI API (Whisper, GPT-4, Embedding)
└── 開発ツール: tsx (hot reload)
```

---

## 📡 API エンドポイント

### メモ関連
| Method | Endpoint | 説明 |
|--------|----------|------|
| GET | `/api/memos` | メモ一覧取得 |
| GET | `/api/memos/:id` | メモ詳細取得 |
| POST | `/api/memos` | テキストメモ作成 |
| POST | `/api/memos/audio` | 音声メモ作成 |
| PATCH | `/api/memos/:id` | メモ更新 |
| DELETE | `/api/memos/:id` | メモ削除 |
| POST | `/api/memos/:id/pin` | ピン留め切り替え |
| GET | `/api/memos/:id/related` | 関連メモ取得 |
| GET | `/api/memos/graph` | ナレッジグラフデータ |
| POST | `/api/memos/similar` | セマンティック検索 |
| GET | `/api/memos/search` | 全文検索 |
| GET | `/api/memos/stats` | 統計情報 |
| GET | `/api/memos/tags` | タグクラウド |

### 認証関連
| Method | Endpoint | 説明 |
|--------|----------|------|
| POST | `/api/auth/register` | ユーザー登録 |
| POST | `/api/auth/login` | ログイン |
| POST | `/api/auth/refresh` | トークンリフレッシュ |
| POST | `/api/auth/apple` | Apple Sign In |

---

## 🔧 技術的ハイライト

### 類似度計算（複合スコア）
```
複合スコア = Embedding類似度 × 0.7 + タグ類似度(Jaccard) × 0.3

関連判定:
- 複合スコア ≥ 0.55
- または Embedding単体 ≥ 0.65
- または タグ類似度 ≥ 0.6
```

### ナレッジグラフ
- **Force-Directed Layout**: 反発力 + 引力の物理シミュレーション
- **サーバーサイド計算**: ノード・エッジ・統計をAPIで返却
- **クラスター検出**: Union-Findアルゴリズム
- **視覚化**: Canvas APIによる効率的描画

### パフォーマンス最適化
- **N+1問題解消**: MongoDB $lookup, バルク操作
- **Redisキャッシュ**: 多層キャッシュ戦略（5分〜30分TTL）
- **Aggregation Pipeline**: 複雑なクエリの効率化
- **ループアンローリング**: コサイン類似度計算の高速化

---

## 📁 ディレクトリ構成

```
EchoLog/
├── 📱 frontend/                           # iOS App (Swift/SwiftUI)
│   ├── Core/                             # コア機能・共通モジュール
│   │   ├── Models/
│   │   │   ├── Memo.swift               # メモモデル
│   │   │   ├── Category.swift           # カテゴリーモデル
│   │   │   ├── GraphData.swift          # グラフデータ構造
│   │   │   ├── EcomokoCharacter.swift   # キャラクターモデル
│   │   │   └── EcomokoState.swift       # キャラクター状態
│   │   ├── Services/
│   │   │   ├── APIService.swift         # API通信基盤
│   │   │   ├── AuthService.swift        # 認証サービス
│   │   │   ├── MemoService.swift        # メモAPI
│   │   │   ├── AudioService.swift       # 音声録音
│   │   │   ├── RecordingService.swift   # リアルタイム録音
│   │   │   ├── OpenAIService.swift      # OpenAI統合
│   │   │   ├── AppleSignInService.swift # Apple認証
│   │   │   ├── EcomokoService.swift     # ランキングAPI
│   │   │   ├── ShareService.swift       # 共有機能
│   │   │   └── SyncManager.swift        # 同期管理
│   │   ├── Utils/
│   │   │   ├── KeychainManager.swift    # トークン保存
│   │   │   ├── NetworkMonitor.swift     # ネットワーク監視
│   │   │   └── HapticManager.swift      # 触覚フィードバック
│   │   ├── Views/
│   │   │   ├── Components.swift         # 共通UIコンポーネント
│   │   │   └── CustomTextFieldStyles.swift
│   │   └── Extensions/
│   │       ├── Color+Extensions.swift   # カラーテーマ
│   │       ├── View+Extensions.swift
│   │       └── Date+Extensions.swift
│   │
│   ├── Features/                         # 機能別モジュール
│   │   ├── Authentication/              # 認証機能
│   │   │   ├── Views/
│   │   │   │   ├── LoginView.swift
│   │   │   │   └── RegisterView.swift
│   │   │   └── ViewModels/
│   │   │       └── AuthViewModel.swift
│   │   │
│   │   ├── Home/                        # ホーム画面
│   │   │   ├── Views/
│   │   │   │   ├── HomeView.swift
│   │   │   │   ├── MemoListView.swift
│   │   │   │   └── NewMemoView.swift
│   │   │   └── ViewModels/
│   │   │       └── HomeViewModel.swift
│   │   │
│   │   ├── MemoDetail/                  # メモ詳細・編集
│   │   │   ├── Views/
│   │   │   │   ├── MemoDetailView.swift
│   │   │   │   └── EditMemoView.swift
│   │   │   └── ViewModels/
│   │   │       └── MemoDetailViewModel.swift
│   │   │
│   │   ├── Categories/                  # カテゴリー管理
│   │   │   ├── Views/
│   │   │   │   ├── CategoriesView.swift
│   │   │   │   └── CategoryDetailView.swift
│   │   │   └── ViewModels/
│   │   │       └── CategoryViewModel.swift
│   │   │
│   │   ├── EchoAssistant/              # ナレッジグラフ
│   │   │   ├── Views/
│   │   │   │   └── KnowledgeGraphView.swift
│   │   │   └── ViewModels/
│   │   │       └── KnowledgeGraphViewModel.swift
│   │   │
│   │   ├── ChatLog/                    # チャットログ
│   │   │   ├── Views/
│   │   │   │   └── ChatLogView.swift
│   │   │   └── ViewModels/
│   │   │       └── ChatLogViewModel.swift
│   │   │
│   │   ├── Ecomoko/                    # キャラクター育成
│   │   │   ├── Views/
│   │   │   │   ├── EcomokoView.swift
│   │   │   │   └── CharacterCollectionView.swift
│   │   │   └── ViewModels/
│   │   │       └── EcomokoViewModel.swift
│   │   │
│   │   └── Recording/                  # 録音機能
│   │       ├── Views/
│   │       │   └── RecordingView.swift
│   │       └── ViewModels/
│   │           └── RecordingViewModel.swift
│   │
│   ├── Persistence/                     # データ永続化
│   │   └── PersistenceController.swift
│   │
│   ├── EchoLogApp/                     # アプリエントリーポイント
│   │   ├── EchoLogApp.swift           # App定義
│   │   ├── ContentView.swift          # ルートビュー
│   │   └── Info.plist
│   │
│   ├── Tests/                          # テスト
│   │   └── EchoLogAppTests/
│   │
│   └── project.yml                     # XcodeGen設定
│
├── 🖥️ backend/                           # API Server (Node.js/TypeScript)
│   ├── src/
│   │   ├── config/
│   │   │   ├── database.ts            # MongoDB接続
│   │   │   └── env.ts                 # 環境変数管理
│   │   │
│   │   ├── controllers/
│   │   │   └── uploadController.ts    # ファイルアップロード
│   │   │
│   │   ├── middleware/
│   │   │   ├── auth.ts                # JWT認証
│   │   │   ├── validate.ts            # Zodバリデーション
│   │   │   └── errorHandler.ts        # エラーハンドリング
│   │   │
│   │   ├── models/
│   │   │   ├── memo.ts                # メモモデル
│   │   │   └── user.ts                # ユーザーモデル
│   │   │
│   │   ├── routes/
│   │   │   ├── auth.ts                # 認証エンドポイント
│   │   │   ├── memos.ts               # メモエンドポイント
│   │   │   ├── ecomoko.ts             # ランキングエンドポイント
│   │   │   ├── gpt.ts                 # GPT統合
│   │   │   ├── whisper.ts             # Whisper統合
│   │   │   └── upload.ts              # アップロード
│   │   │
│   │   ├── services/
│   │   │   ├── memo.service.ts        # メモビジネスロジック
│   │   │   ├── ecomoko.service.ts     # ランキング計算
│   │   │   ├── job.service.ts         # 非同期ジョブ
│   │   │   └── storage.service.ts     # ファイルストレージ
│   │   │
│   │   ├── types/
│   │   │   └── memo.types.ts          # 型定義
│   │   │
│   │   ├── utils/
│   │   │   ├── cache.ts               # メモリキャッシュ
│   │   │   ├── redis.ts               # Redis接続
│   │   │   ├── similarity.ts          # 類似度計算
│   │   │   └── logger.ts              # ロギング
│   │   │
│   │   └── index.ts                   # サーバーエントリーポイント
│   │
│   ├── scripts/                        # 管理スクリプト
│   │   ├── create-test-user.ts
│   │   ├── list-users.ts
│   │   ├── reset-user.ts
│   │   ├── check-db.ts
│   │   └── fix-indexes.ts
│   │
│   ├── uploads/                        # アップロードファイル
│   │   ├── audio/
│   │   └── temp/
│   │
│   ├── package.json
│   ├── tsconfig.json
│   └── .env                           # 環境変数
│
├── 📚 docs/                             # ドキュメント
│   └── docs/
│       ├── ECOMOKO_RANKING_DEVELOPMENT.md    # ランキング開発ドキュメント
│       ├── ECOMOKO_OPERATIONS_GUIDE.md       # 運用ガイド
│       ├── FUTURE_IDEAS.md                   # 今後の実装アイデア
│       ├── PERFORMANCE_OPTIMIZATION.md       # パフォーマンス最適化
│       ├── MEMO_REFACTORING_SUMMARY.md       # リファクタリング記録
│       └── DEVICE_CONNECTION_TROUBLESHOOTING.md
│
├── .github/                            # GitHub設定
│   └── workflows/
│
├── README.md                           # このファイル
├── OAUTH_STATUS.md                     # OAuth実装状況
└── QUICK_FIX.md                        # クイックフィックスガイド
```

### 📂 主要ファイルの役割

#### Frontend
- **Core/Models**: データ構造の定義（Memo, Category, GraphData等）
- **Core/Services**: API通信、認証、音声処理などの共通サービス
- **Features**: 機能ごとにMVVMパターンで分離（View + ViewModel）
- **Persistence**: オフラインデータ、キャッシュ管理

#### Backend
- **routes**: エンドポイント定義（薄いレイヤー）
- **services**: ビジネスロジック（太いレイヤー）
- **models**: MongoDBスキーマ定義
- **middleware**: 認証、バリデーション、エラー処理
- **utils**: 類似度計算、キャッシュ、ロギング等のユーティリティ

#### Docs
- **開発ドキュメント**: 実装詳細、パフォーマンス最適化、運用ガイド
- **アイデア集**: 今後の機能拡張案

---

## 🛠️ セットアップ

### バックエンド
```bash
cd backend
npm install

# 環境変数設定
cp .env.example .env
# MONGODB_URI, OPENAI_API_KEY, JWT_SECRET等を設定

npm run dev
```

### フロントエンド
```bash
cd frontend
xcodegen generate
open EchoLogApp.xcodeproj
```

### 管理スクリプト
```bash
# テストユーザー作成
npm run create-test-user

# ユーザー一覧
npm run list-users

# ユーザー削除
npm run reset-user <email>
```

---

## 📊 データフロー

```
1. 音声録音 / テキスト入力
   ↓
2. Whisper API → 文字起こし（音声の場合）
   ↓
3. GPT-4 → タイトル・タグ自動生成
   ↓
4. OpenAI Embedding → ベクトル化（1536次元）
   ↓
5. MongoDB保存
   ↓
6. 関連メモ計算（Embedding + タグ複合スコア）
   ↓
7. 双方向リンク更新（バルク操作）
   ↓
8. キャッシュ無効化
   ↓
9. ナレッジグラフ表示
```

---

## 📈 パフォーマンス指標

- **グラフ描画**: 200ノード・400エッジまで快適動作
- **レイアウト計算**: 50イテレーション、約100ms
- **API応答**: キャッシュヒット時 < 50ms
- **類似度計算**: バックグラウンド非同期処理

---

## 🔮 今後の拡張案

- [ ] オフラインモード強化
- [ ] リアルタイム文字起こし
- [ ] ウィジェット対応
- [ ] 3Dナレッジグラフ（SceneKit）
- [ ] 音声検索
- [ ] マルチデバイス同期

---

## 👤 開発者

**Towa Yokoyama**
- GitHub: [@TowaYokoyama](https://github.com/TowaYokoyama)

---

**Built with ❤️ using Swift, SwiftUI, Node.js & AI**
