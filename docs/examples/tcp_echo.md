<!-- # TCP echo server -->
# TCP echo サーバー

<!-- ## Concepts -->
## 概念

<!--
- Listening for TCP port connections with
  [Deno.listen](https://doc.deno.land/builtin/stable#Deno.listen).
- Use [Deno.copy](https://doc.deno.land/builtin/stable#Deno.copy) to take
  inbound data and redirect it to be outbound data.
-->
- [Deno.listen](https://doc.deno.land/builtin/stable#Deno.listen) でTCPポートへのコネクションをリッスンする。
- [Deno.copy](https://doc.deno.land/builtin/stable#Deno.copy) を使用し、受信したデータを送信するデータにリダイレクトします。

<!-- ## Example -->
## 例

<!--
This is an example of a server which accepts connections on port 8080, and
returns to the client anything it sends.
-->
これはポート8080でコネクションを受けクライアントから送られてきたものをそのまま返すサーバーの例です。

```ts
/**
 * echo_server.ts
 */
const listener = Deno.listen({ port: 8080 });
console.log("listening on 0.0.0.0:8080");
for await (const conn of listener) {
  Deno.copy(conn, conn);
}
```

<!-- Run with: -->
実行してください:

```shell
deno run --allow-net echo_server.ts
```

<!--
To test it, try sending data to it with
[netcat](https://en.wikipedia.org/wiki/Netcat) (Linux/MacOS only). Below
`'hello world'` is sent over the connection, which is then echoed back to the
user:
-->
テストするには [netcat](https://en.wikipedia.org/wiki/Netcat) (Linux/MacOSのみ)を使用してデータを送信してみてください。下記では `'hello world'` が送信されユーザーにエコーバックされています:

```shell
$ nc localhost 8080
hello world
hello world
```

<!--
Like the [cat.ts example](./unix_cat.md), the `copy()` function here also does
+not make unnecessary memory copies. It receives a packet from the kernel and
+sends back, without further complexity.
-->
[cat.tsの例](./unix_cat.md) と同じように `copy()` 関数は無駄なメモリコピーを作りません。この関数はカーネルからパケットが送られそして複雑なことをせずに送り返します。
