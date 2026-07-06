# CLAUDE.md — Shopify DVDストア 開発ガイド

## プロジェクト概要

西洋時代劇・鉄道車窓DVDを販売する個人EC。Shopify + Horizonテーマベース。

## 技術スタック

- **プラットフォーム**: Shopify (Online Store 2.0)
- **テーマ**: Horizon（無料、OS2.0対応）
- **カスタマイズ**: CSS変数上書き + Liquid セクション編集
- **決済**: Shopify Payments（クレジットカード）
- **言語**: 日本語

## 開発ルール

### テーマカスタマイズ方針
- Horizonの`settings_schema.json`カスタマイズを最優先（コード変更より設定変更を先に試す）
- CSS変更は`assets/custom.css`に集約。テーマファイルを直接書き換えない
- Liquid変更はセクションファイル単位で行い、スニペットは`snippets/`に切り出す
- `layout/theme.liquid`の変更は最小限に留める

### デザイン原則（Netflixダークトーン）
- カラースキーム: ダーク背景（`#141414`）、アクセント赤（`#E50914`）、テキスト白
- 商品画像は16:9またはDVDジャケット比率（約0.7:1）で統一
- フォント: 日本語 Noto Sans JP、見出しは太め（700）

### 在庫管理の注意点
- 商品は最大10枚/タイトル。在庫切れ時の自動購入ブロックを必ず有効化
- Shopify管理画面 > 商品 > 在庫追跡を「Shopifyで追跡」に設定すること
- 残り3枚以下は「残りわずか」表示（Liquidで`variant.inventory_quantity`を参照）

### 運用自動化（最優先）
- 注文確認メール・発送通知メールはShopify標準テンプレートを活用
- 追跡番号はShopify管理画面「注文 > フルフィルメント」から登録
- 手動作業が発生するのは「梱包・発送・追跡番号入力」の3工程のみにする

## ファイル構成メモ

```
sections/
  header.liquid         # ナビゲーション
  featured-product.liquid  # 商品詳細ヒーローセクション
  collection-*.liquid   # 商品一覧
  blog-*.liquid         # ブログ
snippets/
  product-card.liquid   # カードUI（一覧・レコメンド共用）
  low-stock-badge.liquid # 残りわずかバッジ
assets/
  custom.css            # テーマ上書きCSS（ここに集約）
```

## 完成条件

**1件の実際の購入が完了した時点で完成とみなす。**
機能網羅より「買える状態」を最優先に実装する。
