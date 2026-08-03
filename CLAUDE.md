# CLAUDE.md

## このrepoの役割
@moncface/colophon / colophon-spec / @moncface/koine / koine-spec の monorepo。
タグ v* の push が GitHub Actions 経由の npm publish
(OIDC / Trusted Publishing、トークンレス)を起動する。

## publish の承認(必須・唯一のゲート)
**GitHub Environment の保護ルールは外してある。タグを打てば止まらずに publish される。**
したがってタグ操作の前に必ず:
1. 現行版→提案版(例 0.0.4→0.0.5)
2. 変更内容の要約と差分
3. `npm publish --workspaces --dry-run` の結果(tarballに入るファイル一覧)
を tachi-hiro に提示し、明示的な承認を得る。**承認なしのタグ作成・push は違反。**

workflow(.github/workflows/*)を変更する場合は、release.yml の全文差分も提示する。

## 経路が2つある
- **チャットのClaude**: Supabase `release_requests` に INSERT →
  Edge Function `npm-release` が commit + タグ作成
- **Claude Code(ここ)**: 直接 commit + タグ + push

**作業前に必ず `git pull`。** チャット側の publish が main を進めているので、
ローカルが古いまま作業すると衝突する。

## LICENSE は配らない
LICENSE は repo 直下に1本だけ置く。publish 直前に workflow が各パッケージへ
コピーする(`for d in packages/*/; do cp LICENSE "$d"; done`)。
**`packages/*/LICENSE` をコミットしないこと。** clone しただけでは
各パッケージに LICENSE が見えないが、それが正しい状態。

## その他
- 版は 0.x で進める。package@version は焼き付き再利用不可
- 版上げは `npm version <ver> --workspaces --no-git-tag-version`
- node_modules / package-lock.json は commit しない
- publish 直後に npm のパッケージ文書エンドポイントが一時 404 を
  返すことがある。**再 publish しない**(tarball は存在する。待つ)
