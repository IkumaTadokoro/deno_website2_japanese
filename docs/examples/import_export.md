<!-- # Import and export modules -->
# モジュールのインポートとエクスポート

<!-- ## Concepts -->
## 概念

<!--
- [import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
  allows you to include and use modules held elsewhere, on your local file
  system or remotely.
- Imports are URLs or file system paths.
- [export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)
  allows you to specify which parts of your module are accessible to users who
  import your module.
-->
- [import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) はローカルファイルシステムやリモートにあるモジュールをインクルードし使用することをできるようにします。
- importはURLやファイルパスを使います。
- [export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) はそのモジュールをimportするユーザーに対し、モジュールのどの部分をアクセス可能にするか指定することができます。

<!-- ## Overview -->
## 概要

<!--
Deno by default standardizes the way modules are imported in both JavaScript and
TypeScript using the ECMAScript 6 `import/export` standard.
-->
DenoはデフォルトではECMAScript 6の `import/export` 標準を使用してJavaScriptとTypeScriptの療法でモジュールのインポート方法を標準化しています。

<!--
It adopts browser-like module resolution, meaning that file names must be
specified in full. You may not omit the file extension and there is no special
handling of `index.js`.
-->
ブラウザライクなモジュール解決を採用しており、ファイル名は完全に指定しなければいけません。拡張子を省略することはできませんし、`index.js` の特別な扱いもありません。

```js
import { add, multiply } from "./arithmetic.ts";
```

<!--
Dependencies are also imported directly, there is no package management
overhead. Local modules are imported in exactly the same way as remote modules.
As the examples show below, the same functionality can be produced in the same
way with local or remote modules.
-->
依存関係も直接インポートされます、パッケージマネージャのオーバーヘッドはありません。ローカルモジュールはリモートモジュールと全く同じ方法でインポートされます。下記の例のようにローカルモジュールでもリモートモジュールでも同じ方法で同じ機能を再現できます。

<!-- ## Local Import -->
## ローカルインポート

<!--
In this example the `add` and `multiply` functions are imported from a local
`arithmetic.ts` module.
-->
この例では、`add` と `multiply` 関数がローカルの `arithmetic.ts` モジュールからインストールされています。

**Command:** `deno run local.ts`

<!--
```ts
/**
 * local.ts
 */
import { add, multiply } from "./arithmetic.ts";

function totalCost(outbound: number, inbound: number, tax: number): number {
  return multiply(add(outbound, inbound), tax);
}

console.log(totalCost(19, 31, 1.2));
console.log(totalCost(45, 27, 1.15));

/**
 * Output
 *
 * 60
 * 82.8
 */
```
-->
```ts
import { add, multiply } from "./arithmetic.ts";

function totalCost(outbound: number, inbound: number, tax: number): number {
  return multiply(add(outbound, inbound), tax);
}

console.log(totalCost(19, 31, 1.2));
console.log(totalCost(45, 27, 1.15));

/**
 * 出力
 *
 * 60
 * 82.8
 */
```

<!-- ## Remote Import -->
## リモートインポート

<!--
In the local import example above an `add` and `multiply` method are imported
from a locally stored arithmetic module. The same functionality can be created
by importing `add` and `multiply` methods from a remote module too.
-->
上記の `add` と `multiply` メソッドのインポートの例はローカルに本尊されたarithmeticモジュールからインポートされました。リモートモジュールから `add` と `multiply` メソッドをインポートしても同じ機能を作ることが出来ます。

<!--
In this case the Ramda module is referenced, including the version number. Also
note a JavaScript module is imported directly into a TypeScript module, Deno has
no problem handling this.
-->
この例ではRamdaモジュールがバージョン付きで参照されています。また、JavaScriptモジュールがTypeScriptモジュールに直接インポートされていることに注意してください。Denoはこれを問題なく扱います。

**Command:** `deno run ./remote.ts`

<!--
```ts
/**
 * remote.ts
 */
import {
  add,
  multiply,
} from "https://x.nest.land/ramda@0.27.0/source/index.js";

function totalCost(outbound: number, inbound: number, tax: number): number {
  return multiply(add(outbound, inbound), tax);
}

console.log(totalCost(19, 31, 1.2));
console.log(totalCost(45, 27, 1.15));

/**
 * Output
 *
 * 60
 * 82.8
 */
```
-->
```ts
/**
 * remote.ts
 */
import {
  add,
  multiply,
} from "https://x.nest.land/ramda@0.27.0/source/index.js";

function totalCost(outbound: number, inbound: number, tax: number): number {
  return multiply(add(outbound, inbound), tax);
}

console.log(totalCost(19, 31, 1.2));
console.log(totalCost(45, 27, 1.15));

/**
 * 出力
 *
 * 60
 * 82.8
 */
```

<!-- ## Export -->
## エクスポート

<!--
In the example above the `add` and `multiply` functions are imported from a
locally stored arithmetic module. To make this possible the functions stored in
the arithmetic module must be exported.
-->
上記の `add` と `multiply` 関数の例はローカルからインポートされarithmeticモジュールに保存されました。これを可能にするためにはarithmeticモジュールに保存されている関数はエクスポートする必要があります。

<!--
To do this just add the keyword `export` to the beginning of the function
signature as is shown below.
-->
このためには下記の例のように関数のシグニチャのはじめに `export` を追加するだけです。

```ts
/**
 * arithmetic.ts
 */
export function add(a: number, b: number): number {
  return a + b;
}

export function multiply(a: number, b: number): number {
  return a * b;
}
```

<!--
All functions, classes, constants and variables which need to be accessible
inside external modules must be exported. Either by pretending them with the
`export` keyword or including them in an export statement at the bottom of the
file.
-->
外部モジュールの内部からアクセスする必要があるすべての関数、クラス、定数そして変数はエクスポートされる必要があります。このために `export` をつけるかファイルの最後のエクスポート文に含める必要があります。

<!--
To find out more on ECMAScript Export functionality please read the
[MDN Documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export).
-->
ECMAScriptのエクスポート機能のより詳しい情報は [MDN ドキュメント](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Statements/export) を読んでください。
