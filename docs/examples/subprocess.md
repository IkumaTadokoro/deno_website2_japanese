<!-- # Creating a subprocess -->
# サブプロセスの作成

<!-- ## Concepts -->
## 概念

<!--
- Deno is capable of spawning a subprocess via
  [Deno.run](https://doc.deno.land/builtin/stable#Deno.run).
- `--allow-run` permission is required to spawn a subprocess.
- Spawned subprocesses do not run in a security sandbox.
- Communicate with the subprocess via the
  [stdin](https://doc.deno.land/builtin/stable#Deno.stdin),
  [stdout](https://doc.deno.land/builtin/stable#Deno.stdout) and
  [stderr](https://doc.deno.land/builtin/stable#Deno.stderr) streams.
-->
- Deno [Deno.run](https://doc.deno.land/builtin/stable#Deno.run) を通してサブプロセスを生成することができます。
- `--allow-run` パーミッションはサブプロセスの生成に必要です。
- 生成されたサブプロセスはセキュリティサンドボックス内で実行されません。
- サブプロセスとの通信は [stdin](https://doc.deno.land/builtin/stable#Deno.stdin)、[stdout](https://doc.deno.land/builtin/stable#Deno.stdout)、[stderr](https://doc.deno.land/builtin/stable#Deno.stderr) ストリームを通して行います。
- Use a specific shell by providing its path/name and its string input switch,
  e.g. `Deno.run({cmd: ["bash", "-c", '"ls -la"']});

<!-- ## Simple example -->
## 簡単な例

<!-- This example is the equivalent of running `'echo hello'` from the command line. -->
この例はコマンドラインから `'echo hello'` を実行しているのと同等です。

<!--
```ts
/**
 * subprocess_simple.ts
 */

// create subprocess
const p = Deno.run({
  cmd: ["echo", "hello"],
});

// await its completion
await p.status();
```
-->
```ts
/**
 * subprocess_simple.ts
 */

// サブプロセスの作成
const p = Deno.run({
  cmd: ["echo", "hello"],
});

// 完了を待つ
await p.status();
```

<!-- Run it: -->
実行:

```shell
$ deno run --allow-run ./subprocess_simple.ts
hello
```

<!-- ## Security -->
## セキュリティ

<!--
The `--allow-run` permission is required for creation of a subprocess. Be aware
that subprocesses are not run in a Deno sandbox and therefore have the same
permissions as if you were to run the command from the command line yourself.
-->
サブプロセスの作成には `--allow-run` パーミッションが必要です。サブプロセスはDenoサンドボックスで実行されないためコマンドラインからコマンドを実行するのと同じ権限を持っていることに注意してください。

<!-- ## Communicating with subprocesses -->
## サブプロセスとの通信

<!--
By default when you use `Deno.run()` the subprocess inherits `stdin`, `stdout`
and `stderr` of the parent process. If you want to communicate with started
subprocess you can use `"piped"` option.
-->
デフォルトでは `Deno.run()` サブプロセスを使うとき親プロセスの `stdin`、`stdout` そして `stderr` を継承します。もし開始されたサブプロセスと通信したいときは `"piped"` オプションを使うことが出来ます。

```ts
/**
 * subprocess.ts
 */
const fileNames = Deno.args;

const p = Deno.run({
  cmd: [
    "deno",
    "run",
    "--allow-read",
    "https://deno.land/std@$STD_VERSION/examples/cat.ts",
    ...fileNames,
  ],
  stdout: "piped",
  stderr: "piped",
});

const { code } = await p.status();

if (code === 0) {
  const rawOutput = await p.output();
  await Deno.stdout.write(rawOutput);
} else {
  const rawError = await p.stderrOutput();
  const errorString = new TextDecoder().decode(rawError);
  console.log(errorString);
}

Deno.exit(code);
```

<!-- When you run it: -->
実行時:

```shell
$ deno run --allow-run ./subprocess.ts <somefile>
[file content]

$ deno run --allow-run ./subprocess.ts non_existent_file.md

Uncaught NotFound: No such file or directory (os error 2)
    at DenoError (deno/js/errors.ts:22:5)
    at maybeError (deno/js/errors.ts:41:12)
    at handleAsyncMsgFromRust (deno/js/dispatch.ts:27:17)
```
