# はじめに
最近になってようやくgitでのコミット、プッシュのコマンドを暗記したのですが長いですね。

```zsh
git add --all; git commit -m 'first commit'; git push --all
```

これを関数化しました。

簡単なコミット方法を身近なエンジニアに聞いてみましたが「エイリアス化じゃね？」ということだったのでエイリアス化して、その後でコミットメッセージを付けるために関数化しました。

# 使い方

```zsh
$ ga
```

と打つだけでadd, コミット、プッシュをしてくれます。
このままだとコミットメッセージが自動で付いてしまいますが、`-m`オプションを付けるとコミットメッセージも指定出来ます。

```zsh
$ ga -m 'hoge message'
```

# インストール？方法

コードです。

```zsh:git-add-all-git-commit-git-push-all.zsh
#!/usr/bin/env zsh

# git add --all; git commit -m 'first commit'; git push --allgit add --all; git commit -m 'first commit'; git push --all をしてくれるコマンド

# Thanks @link http://qiita.com/mollifier/items/eba71bc33bb7e3b76233
# Thanks @link http://qiita.com/petitviolet/items/b1e8b5139169dd530919
# Thanks @link http://qiita.com/mollifier/items/6fdeff2750fe80f830c8

# Usage: ga [-m|--message message]
function ga() {
  local -A opthash
  local MESSAGE
  zparseopts -D -A opthash -- -help h -message: m:
  if [[ -n "${opthash[(i)--help]}" ]]; then
    echo 'Usage: ga [-m|--message message]'
    return 0
  fi
  if [[ -n "${opthash[(i)-h]}" ]]; then
    echo 'Usage: ga [-m|--message message]'
    return 0
  fi

  if [[ -n "${opthash[(i)--message]}" ]]; then
     MESSAGE=${opthash[--message]}
  fi
  if [[ -n "${opthash[(i)-m]}" ]]; then
     MESSAGE=${opthash[-m]}
  fi
  if [[ $MESSAGE ]] then
     CMD="git add --all; git commit -m '${MESSAGE}'; git push --all"
  else
     CMD="git add --all; git commit -m 'Automatically commit'; git push --all"
  fi
  eval ${CMD}
}

_gacmd() {
  _arguments \
  -m'[commit message]' \
  --message'[commit message]'
}

compdef _gacmd ga
```

ファイルを作って

```zsh
$ source git-add-all-git-commit-git-push-all.zsh
```
sourceで読ませます。

でもsourceは良くないらしいので、将来的には他の方法にしたいです。

> zsh で source して使うプラグインを作るのは止めにしよう
> http://qiita.com/mollifier/items/6fdeff2750fe80f830c8

僕のレベルではまだよく分かりませんでした。

githubにコードを置いてみました。名前が長いのは当てつけです。
> https://github.com/yousan/git-add-all-git-commit-git-push-all