# CLAUDE.md

## このrepoの役割
@moncface/colophon / colophon-spec / @moncface/koine / koine-spec の monorepo。
タグ v* の push が GitHub Actions 経由の npm publish
(OIDC / Trusted Publishing、トークンレス)を起動する。

## publish の二段承認(必須)
1. **承認①(版上げ承認)**: タグ操作の前に、現行版→提案版(例 0.0.2→0.0.3)、
   変更内容の要約、tarball に入るファイル一覧
   (`npm publish --workspaces --dry-run` の結果)を tachi-hiro に提示し、
   明示的な承認を得る。**承認なしのタグ作成・push は違反**
2. **承認②(構造)**: タグ後、Actions の publish ジョブは
   Environment `npm-publish` で停止する。tachi-hiro が Approve して
   初めて publish される

## 通常経路
版上げとタグ作成は通常、チャット側の Claude → Supabase `release_requests`
→ Edge Function `npm-release` が行う。この repo を直接触るのは
構築・修理・workflow 変更の時だけ。

## その他
- 版は 0.x で進める。package@version は焼き付き再利用不可
- 版上げは `npm version <ver> --workspaces --no-git-tag-version`
- node_modules / package-lock.json は commit しない
- publish 直後に npm のパッケージ文書エンドポイントが一時 404 を
  返すことがある。**再 publish しない**(tarball は存在する。待つ)
