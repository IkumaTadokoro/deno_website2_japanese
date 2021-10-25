<!-- ## Set up your environment -->
## 環境構築

<!--
To productively get going with Deno you should set up your environment. This
means setting up shell autocomplete, environmental variables and your editor or
IDE of choice.
-->
Denoの開発で生産性を上げるために環境構築すべきです。つまり。シェル自動補完や環境変数、好きなエディターやIDEの設定をすることです。

<!-- ### Environmental variables -->
### 環境変数

<!-- There are several env vars that control how Deno behaves: -->
Denoがどのように動作するか決める環境変数がいくつかあります。

<!--
`DENO_DIR` defaults to `$HOME/.cache/deno` but can be set to any path to control
where generated and cached source code is written and read to.
-->
`DENO_DIR` のデフォルトは `$HOME/.cache/deno` ですが、生成されキャッシュされたソースコードがどこに書き込まれどこから読まれるかは任意のパスに設定することが出来ます。

<!--
`NO_COLOR` will turn off color output if set. See https://no-color.org/. User
code can test if `NO_COLOR` was set without having `--allow-env` by using the
boolean constant `Deno.noColor`.
-->
`NO_COLOR` がセットされていればは出力の色付けを切ります。https://no-color.org/ を参照してください。ユーザーコードはブール定数 `Deno.noColor` を使うことで `--allow-env` を用いなくても `NO_COLOR` がセットされているかどうか調べることが出来ます。

<!-- ### Shell autocomplete -->
### シェル自動補完

<!--
You can generate completion script for your shell using the
`deno completions <shell>` command. The command outputs to stdout so you should
redirect it to an appropriate file.
-->
`deno completions <shell>` コマンドを使うことでシェルの補完スクリプトを生成することが出来ます。このコマンドは標準出力に出力するので適切なファイルにリダイレクトしてください。

<!-- The supported shells are: -->
サポートされているシェルは:

- zsh
- bash
- fish
- powershell
- elvish

<!-- Example (bash): -->
例 (bash):

```shell
deno completions bash > /usr/local/etc/bash_completion.d/deno.bash
source /usr/local/etc/bash_completion.d/deno.bash
```

<!-- Example (zsh without framework): -->
例 (zsh フレームワークなし):

```shell
mkdir ~/.zsh # create a folder to save your completions. it can be anywhere
deno completions zsh > ~/.zsh/_deno
```

<!-- then add this to your `.zshrc` -->
`.zshrc` に加えてください

```shell
fpath=(~/.zsh $fpath)
autoload -Uz compinit
compinit -u
```

<!--
and restart your terminal. note that if completions are still not loading, you
may need to run `rm ~/.zcompdump/` to remove previously generated completions
and then `compinit` to generate them again.
-->
そしてターミナルをリスタートしてください。もしまだ補完がロードされない場合は、以前に作成された補完を削除するために `rm ~/.zcompdump/` を実行し、もう一度 `compinit` で作成してください。

<!-- Example (zsh + oh-my-zsh) [recommended for zsh users] : -->
例 (zsh + oh-my-zsh) [zshユーザーにおすすめ]:

```shell
mkdir ~/.oh-my-zsh/custom/plugins/deno
deno completions zsh > ~/.oh-my-zsh/custom/plugins/deno/_deno
```

<!--
After this add deno plugin under plugins tag in `~/.zshrc` file. for tools like
`antigen` path will be `~/.antigen/bundles/robbyrussell/oh-my-zsh/plugins` and
command will be `antigen bundle deno` and so on.
-->
これは `deno` プラグインを `~/.zshrc` のプラグインタグに追加します。`antigen` のようなツールのパスは `~/.antigen/bundles/robbyrussell/oh-my-zsh/plugins` になりコマンドは `antigen bundle deno` になります。

<!-- Example (Powershell): -->
例 (Powershell):

```shell
deno completions powershell >> $profile
.$profile
```

<!--
This will be create a Powershell profile at
`$HOME\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1` by default,
and it will be run whenever you launch the PowerShell.
-->
これはデフォルトで `$HOME\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1` にPowershellプロフィールを作り、Powershellを実行するときに自動で実行されます。

<!-- ### Editors and IDEs -->
### エディターとIDE

<!--
Because Deno requires the use of file extensions for module imports and allows
http imports, and most editors and language servers do not natively support this
at the moment, many editors will throw errors about being unable to find files
or imports having unnecessary file extensions.
-->
Denoはモジュールのインポートに拡張子を使用する必要があり、HTTPインポートを許可していますが、ほとんどののエディターやランゲージサーバーは現段階でネイティブにこれらをサポートしていません。多くのエディターはファイルが見つからないやインポート時の不執拗な拡張子などでエラーを出すでしょう。

<!-- The community has developed extensions for some editors to solve these issues: -->
コミュニティはこれらの問題を解決するためにいくつかのエディターには拡張機能を開発しました:

#### VS Code

<!--
The beta version of [vscode_deno](https://github.com/denoland/vscode_deno) is
published on the
[Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=denoland.vscode-deno).
Please report any issues.
-->
ベータ版の [vscode_deno](https://github.com/denoland/vscode_deno) は [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=denoland.vscode-deno) で公開されています。問題が発生したらレポートを送ってください。

#### JetBrains IDEs

<!--
Support for JetBrains IDEs is available through
[the Deno plugin](https://plugins.jetbrains.com/plugin/14382-deno).
-->
JetBrains IDEへのサポートは [the Deno plugin](https://plugins.jetbrains.com/plugin/14382-deno) にあります。

<!--
Once installed, replace the content of
`External Libraries > Deno Library > lib > lib.deno.d.ts` with the output of
`deno types`. This will ensure the typings for the extension match the current
version. You will have to do this every time you update the version of Deno. For
more information on how to set-up your JetBrains IDE for Deno, read
[this comment](https://youtrack.jetbrains.com/issue/WEB-41607#focus=streamItem-27-4160152.0-0)
on YouTrack.
-->
インストールしたら、`External Libraries > Deno Library > lib > lib.deno.d.ts` の内容を、`deno types` の出力で置き換えてください。これにより、拡張機能のタイピングが現在のバージョンと一致するようになります。Denoのバージョンをアップデートするたびに、この作業を行う必要があります。Deno 用に JetBrains IDE をセットアップする方法の詳細については、YouTrackの [このコメント](https://youtrack.jetbrains.com/issue/WEB-41607#focus=streamItem-27-4160152.0-0) をお読みください。

<!-- #### Vim and NeoVim -->
#### VimとNeoVim

<!--
Vim works fairly well for Deno/TypeScript if you install
[CoC](https://github.com/neoclide/coc.nvim) (intellisense engine and language
server protocol) or [ALE](https://github.com/dense-analysis/ale) (syntax checker
and language server protocol client).
-->
[CoC](https://github.com/neoclide/coc.nvim) (インテリセンスエンジンと言語サーバープロトコル) か [ALE](https://github.com/dense-analysis/ale) (構文チェッカーと言語サーバープロトコルクライアント) をインストールすれば、Deno/TypeScript上でvimを使うことができます。

##### CoC

<!--
After CoC is installed, from inside Vim, run`:CocInstall coc-tsserver` and
`:CocInstall coc-deno`. Run `:CocCommand deno.initializeWorkspace` in your
+project to initialize workspace configurations. From now on, things like `gd`
(go to definition) and `gr` (goto/find references) should work.
-->
CoCをインストールした後は、vim側から`:CocInstall coc-tsserver`と`:CocInstall coc-deno`を実行させてみましょう。
ワークスペースの設定を初期化するために `:CocCommand deno.initializeWorkspace` をプロジェクトで実行してください。これより、`gd` (go to definition) と `gr` (goto/find references) が動くはずです。

##### ALE

<!--
ALE integrates with Deno's LSP out of the box and should not require any extra
configuration. However, if your Deno executable is not located in `$PATH`, has a
different name than `deno` or you want to use unstable features/APIs, you need
to override ALE's default values. See
[`:help ale-typescript`](https://github.com/dense-analysis/ale/blob/master/doc/ale-typescript.txt).
-->

<!--
ALE provides support for autocompletion, refactoring, going to definition,
finding references and more, however, key bindings need to be configured
manually. Copy the snippet below into your `vimrc`/`init.vim` for basic
configuration or consult the
[official documentation](https://github.com/dense-analysis/ale#table-of-contents)
for a more in-depth look at how to configure ALE.
-->
ALE は自動補完、リファクタリング、定義への移動、参照の検索などをサポートしていますが、キーバインディングは手動で設定する必要があります。以下のスニペットを `vimrc`/`init.vim` にコピーして基本的な設定を行うか、ALE の設定方法については [公式ドキュメント](https://github.com/dense-analysis/ale#table-of-contents) を参照してください。

<!--
ALE can fix linter issues by running `deno fmt`. To instruct ALE to use the Deno
formatter the `ale_linter` setting needs to be set either on a per buffer basis
(`let b:ale_linter = ['deno']`) or globally for all TypeScript files
(`let g:ale_fixers={'typescript': ['deno']}`)
-->
ALE は `deno fmt` を実行することでリンタの問題を修正することができます。ALE に Deno フォーマッタを使うように指示するには、`ale_linter` の設定をバッファごとに設定するか (`let b:ale_linter = ['deno']`)、またはすべての TypeScript ファイルに対してグローバルに設定する必要があります (`let g:ale_fixers={'typescript': ['deno']}`)。

```vim
" Use ALE autocompletion with Vim's 'omnifunc' setting (press <C-x><C-o> in insert mode)
autocmd FileType typescript set omnifunc=ale#completion#OmniFunc

" Make sure to use map instead of noremap when using a <Plug>(...) expression as the {rhs}
nmap gr <Plug>(ale_rename)
nmap gR <Plug>(ale_find_reference)
nmap gd <Plug>(ale_go_to_definition)
nmap gD <Plug>(ale_go_to_type_definition)

let g:ale_fixers = {'typescript': ['deno']}
let g:ale_fix_on_save = 1 " run deno fmt when saving a buffer
```

#### Emacs

<!--
Emacs works pretty well for a TypeScript project targeted to Deno by using a
combination of [tide](https://github.com/ananthakumaran/tide) which is the
canonical way of using TypeScript within Emacs and
[typescript-deno-plugin](https://github.com/justjavac/typescript-deno-plugin)
which is what is used by the
[official VSCode extension for Deno](https://github.com/denoland/vscode_deno).
-->
EmacsはEmacsでのTypeScriptの標準的な使い方である [tide](https://github.com/ananthakumaran/tide) と [official VSCode extension for Deno](https://github.com/denoland/vscode_deno) で使われている [typescript-deno-plugin](https://github.com/justjavac/typescript-deno-plugin) の組み合わせを使うことでDenoでのTypeScriptプロジェクトでうまく動作します。

<!--
To use it, first make sure that `tide` is setup for your instance of Emacs.
Next, as instructed on the
[typescript-deno-plugin](https://github.com/justjavac/typescript-deno-plugin)
page, first `npm install --save-dev typescript-deno-plugin typescript` in your
project (`npm init -y` as necessary), then add the following block to your
`tsconfig.json` and you are off to the races!
-->
これを使うためにまずEmacsのインスタンスに `tide` が設定されていることを確認してください。次に、[typescript-deno-plugin](https://github.com/justjavac/typescript-deno-plugin) のページにあるように、まず `npm install --save-dev typescript-deno-plugin typescript` をプロジェクト内で実行してください(`npm init -y` を必要です)、そして次のコードを `tsconfig.json` に追加してください、これですぐに始めることが出来ます!

```jsonc
{
  "compilerOptions": {
    "plugins": [
      {
        "name": "typescript-deno-plugin",
        "enable": true, // default is `true`
        "importmap": "import_map.json"
      }
    ]
  }
}
```

#### Atom

Install [atom-ide-base](https://atom.io/packages/atom-ide-base) package and
[atom-ide-deno](https://atom.io/packages/atom-ide-deno) package on Atom.

<!-- #### LSP clients -->
#### LSP クライアント

<!--
Deno has builtin support for the
[Language server protocol](https://langserver.org) as of version 1.6.0 or later.
-->
Deno はバージョン1.6.0以降 [Language server protocol](https://langserver.org) へビルトインサポートしています

<!--
If your editor supports the LSP, you can use Deno as a language server for
TypeScript and JavaScript.
-->
もしエディターが LSP をサポートしている場合、Deno を TypeScript と JavaScript の language server として使用することができます。

<!-- The editor can start the server with `deno lsp`. -->
エディターは `deno lsp` でサーバーを開始することができます。

<!-- ##### Example for Kakoune -->
#### Kokoune での例

<!--
After installing the [`kak-lsp`](https://github.com/kak-lsp/kak-lsp) LSP client
you can add the Deno language server by adding the following to your
`kak-lsp.toml`
-->
[`kak-lsp`](https://github.com/kak-lsp/kak-lsp) LSP クライアントをインストールした後、`kak-lsp.toml` に以下を追加することで、Deno language server を追加することができます

```toml
[language.deno]
filetypes = ["typescript", "javascript"]
roots = [".git"]
command = "deno"
args = ["lsp"]

[language.deno.initialization_options]
enable = true
lint = true
```

<!-- ##### Example for Vim/Neovim -->
#### Vim/Neovim での例

<!--
After installing the [`vim-lsp`](https://github.com/prabirshrestha/vim-lsp) LSP
client you can add the Deno language server by adding the following to your
`vimrc`/`init.vim`:
-->
[`vim-lsp`](https://github.com/prabirshrestha/vim-lsp) LSPクライアントをインストールした後、`vimrc`/`init.vim` に以下を追加することで、Deno language server を追加することができます:

```vim
if executable("deno")
  augroup LspTypeScript
    autocmd!
    autocmd User lsp_setup call lsp#register_server({
    \ "name": "deno lsp",
    \ "cmd": {server_info -> ["deno", "lsp"]},
    \ "root_uri": {server_info->lsp#utils#path_to_uri(lsp#utils#find_nearest_parent_file_directory(lsp#utils#get_buffer_path(), "tsconfig.json"))},
    \ "allowlist": ["typescript", "typescript.tsx"],
    \ "initialization_options": {
    \     "enable": v:true,
    \     "lint": v:true,
    \     "unstable": v:true,
    \   },
    \ })
  augroup END
endif
```

<!-- ##### Example for Sublime Text -->
##### Sublime Text での例

<!--
- Install the [Sublime LSP package](https://packagecontrol.io/packages/LSP)
- Install the
  [TypeScript package](https://packagecontrol.io/packages/TypeScript) to get
  syntax highlighting
- Add the following `.sublime-project` file to your project folder
-->
- [Sublime LSP package](https://packagecontrol.io/packages/LSP) をインストールしてください
- シンタックスハイライトのため [TypeScript package](https://packagecontrol.io/packages/TypeScript) をインストールしてください
- 以下の `.sublime-project` ファイルをプロジェクトフォルダーに追加してください

```jsonc
{
  "settings": {
    "LSP": {
      "deno": {
        "command": [
          "deno",
          "lsp"
        ],
        "initializationOptions": {
          // "config": "", // Sets the path for the config file in your project
          "enable": true,
          // "importMap": "", // Sets the path for the import-map in your project
          "lint": true,
          "unstable": false
        },
        "enabled": true,
        "languages": [
          {
            "languageId": "javascript",
            "scopes": ["source.js"],
            "syntaxes": [
              "Packages/Babel/JavaScript (Babel).sublime-syntax",
              "Packages/JavaScript/JavaScript.sublime-syntax"
            ]
          },
          {
            "languageId": "javascriptreact",
            "scopes": ["source.jsx"],
            "syntaxes": [
              "Packages/Babel/JavaScript (Babel).sublime-syntax",
              "Packages/JavaScript/JavaScript.sublime-syntax"
            ]
          },
          {
            "languageId": "typescript",
            "scopes": ["source.ts"],
            "syntaxes": [
              "Packages/TypeScript-TmLanguage/TypeScript.tmLanguage",
              "Packages/TypeScript Syntax/TypeScript.tmLanguage"
            ]
          },
          {
            "languageId": "typescriptreact",
            "scopes": ["source.tsx"],
            "syntaxes": [
              "Packages/TypeScript-TmLanguage/TypeScriptReact.tmLanguage",
              "Packages/TypeScript Syntax/TypeScriptReact.tmLanguage"
            ]
          }
        ]
      }
    }
  }
}
```

<!--
If you don't see your favorite IDE on this list, maybe you can develop an
extension. Our [community Discord group](https://discord.gg/deno) can give you
some pointers on where to get started.
-->
もしお気に入りのIDEがこのページにない場合、拡張機能を作ることが出来ます。[community Discord group](https://discord.gg/deno) でどこから始めればよいかを学ぶことが出来ます
