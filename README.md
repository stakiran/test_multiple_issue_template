# test_multiple_issue_template
GitHub で複数テンプレートを選べる Multiple Issue Templates について仕様を調べてみた
Github で日本語を扱うことが多いので、細かい仕様や挙動を一通り調べてみた。ついでに関連情報もまとめた。

サンプルリポジトリ: [stakiran/test_multiple_issue_template](https://github.com/stakiran/test_multiple_issue_template/)

## GitHub ブログのアナウンス
- 18/05/02 [Issue template improvements | The GitHub Blog](https://blog.github.com/2018-05-02-issue-template-improvements/)

GUI として提供され始めたのは 2018/05/02 っぽい。

- 18/01/25 [Multiple issue and pull request templates | The GitHub Blog](https://blog.github.com/2018-01-25-multiple-issue-and-pull-request-templates/)

しかし機能自体は 2018/01/25 時点では存在していた模様。 `/issues/new?template=bugs.md` みたいに使いたいテンプレートを URL で指定する感じ。

## multiple issue templates?
この機能の呼び方だけど、 [multiple issue templates](https://blog.github.com/2018-05-02-issue-template-improvements/) でいいのかしら。

> We recently helped project maintainers set up multiple issue templates as a way to manage contributions, 

## ディレクトリ構成/ファイル構成
ディレクトリ構成:

- .github/ISSUE_TEMPLATE/XXXX.md

XXXX.md の中身:

```
---
name: <テンプレート名>
about: <テンプレートの説明>

---

<ここからテンプレート本文>
...
...
```

XXXX.md のファイル名について:

- **New Issue 画面での表示順** に影響する(後述)

## New Issue 時にどう表示されるか
![multiple_issue_filenames](https://user-images.githubusercontent.com/23325839/41838419-5d9fca8a-789b-11e8-8157-cf2e490be8a2.jpg)

↓

![multiple_issue_new_buttons](https://user-images.githubusercontent.com/23325839/41838420-5dceb084-789b-11e8-8634-d2c69fff31c7.jpg)

- `.github/ISSUE_TEMPLATE/XXXX.md` の内容がファイル名降順（リポジトリのファイル表示順序と同じ）でズラリと並ぶ
- XXXX として日本語ファイル名を使うと、New Issue から作ろうとした時に **Ooops! 500 エラーページ** が出る

500 エラー:

![multiple_issue_500_error](https://user-images.githubusercontent.com/23325839/41838418-5d6993e8-789b-11e8-9fd2-80c9fac5bda1.jpg)

## .github/ISSUE_TEMPLATE 配下を GitHub に作ってもらう
一から作るより楽できるかもしれない。

作り方:

- Settings > Features > Get organized with issue templates > Set up templates ボタン
- 下部の Add template から適当に選び、追加された行に対して Preview and edit ボタンを押して中身を編集、終わったら Close preview ボタンで保存
  - Name: テンプレート名
  - About: テンプレートの説明
  - Template content: テンプレートの中身

日本語が絡む注目すべき仕様:

- **テンプレート名(Name)に日本語が含まれる** 場合、`-` に置換される
  - `テンプレート１.md` → `-------.md`
- ハイフン置換によりファイル名が被った場合、後の方が優先される
  - `テンプレート.md` と `ほげほげほげ.md` がある場合、`------.md` は「ほげほげほげ.md」の方を指す

## まとめ
- New Issue 画面での表示順はファイル名降順
- ISSUE_TEMPLATE 配下に日本語ファイル名を置いちゃいけない
