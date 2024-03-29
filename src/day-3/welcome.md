# Day3用ページ

## このページの目的

「【G's EXPANSION】 爆速で身に着けるRuby on Rails：自分だけのCMSを開発しよう！」の最終日Day3の講義用ページです。

### 今日の講義の目的

1. 課題発表と振り返り会

2. 各自でハンズオンいただく


## 課題発表会

皆さんが実装した機能について発表していただきたく思います。

- 実装した背景

- 実現したい機能の要件

- 実際に実装した機能をデモしていただく

- こだわった点、質問したい点などあれば質問していただく

# 各自でハンズオン

本日分は、以下のブランチに上げています。

ハンズオンでは皆さんに準備いただくブランチを使用するので、pullしなくて大丈夫です。

[https://github.com/matakitanakajp09/gs-expansion-rails-sample/tree/lesson-day-3](https://github.com/matakitanakajp09/gs-expansion-rails-sample/tree/lesson-day-3)

## 短縮URL機能

皆さんに実装してみたい機能を実装いただきましたので、自分も「短縮URL機能」を実装してみました。

## 機能要件

- 管理画面で、URLを短縮することができます
- 短縮URLをクリックすると、外部サイトに遷移する確認画面が表示されます
- 確認画面で「遷移する」ボタンを押下すると、外部サイトに遷移します
- インプレッション数、クリック数のカウントをすることができます
- インプレッション数、クリック数は、Redisを用いて一時保存し、定期的にDBできるように、バッチ処理を実装します
- 短縮URLの出来上がりは、以下のようになります

| | URL |
|-----|-----|
| 短縮前 | https://www.kidsweekend.jp/portal |
| 短縮後 | http://localhost:3000/l/e09i1Zxhye |

## 各自でハンズオン

最終日は皆さんに資料見ながらハンズオンをしていただきます（講義形式ではなくこの講義時間で皆さん自身が自分で行なっていく形式）。
随時質問は受け付けますので、わからないことがあれば遠慮なく質問してください。

## ハンズオン用ブランチの準備

`lesson-day-3`というブランチを切ります。ブランチ名はお好きなようにつけていただいて大丈夫です。

```bash
git switch -c lesson-day-3
```

## 短縮URL機能、短縮URLトラッキング機能の実装

### 概要図

![img](./images/short_url_6.jpg)

### 各ハンズオン

- [短縮URL機能](./short-url.md)
- [短縮URLトラッキング機能](./short-url-tracking.md)
