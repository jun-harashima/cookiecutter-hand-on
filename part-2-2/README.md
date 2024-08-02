# Part 2-2. テンプレートの追従

Part 1-2 で自分のテンプレートを作成しました。これで、いくらでも、テンプレートからプロジェクトを生成できます。

しかし、テンプレートの作成に終わりはありません。以下のように、テンプレートを変更したくなる場合があります。

- テンプレート内の Docker イメージや RPM パッケージを更新したくなった
- テンプレート内のバグやタイポを修正したくなった

このような場合、どうすればいいのでしょうか？生成済みの各プロジェクトを一つずつ修正していくしかないのでしょうか？

cruft [^1] を利用すれば、テンプレートの変更に追従できます。まず、ご自身の好みや環境に合わせて、cruft をインストールしてください。cookiecutter と同様、Mac の場合、brew でもインストールできるようです。

生成済みのプロジェクトに cruft を導入する場合、以下のように、生成元のテンプレートにリンクします。Part 1-2 で生成したプロジェクト（merge_sort）にテンプレート（cookiecutter-mypackage）をリンクしてみましょう。

```Shell
$ cd merge_sort/

$ cruft link https://github.com/jun-harashima/cookiecutter-mypackage
  [1/3] project_name (My Project):
```

`cruft link` コマンドの引数には GitHub の URL を指定してください。例として jun-harashima のレポジトリを指定しましたが、みなさんはご自身のレポジトリを指定してください。

`cruft link` コマンドを実行すると、プロジェクト生成時と同じプロンプトが表示されます。生成時と同じ値を入力してください。

```Shell
Linking against the commit: 680228d6b7471a47506c9035e93489b2bcc39cab which corresponds with the git reference: None
Press enter to link against this commit or provide an alternative commit.
```

最後に、リンクするコミットを入力するプロンプトが表示されます。特に問題がなければ、スキップしてください。

そうすると、以下のような .cruft.json が生成されます。

```JSON
{
  "template": "git@github.com:jun-harashima/cookiecutter-mypackage.git",
  "commit": "680228d6b7471a47506c9035e93489b2bcc39cab",
  "checkout": null,
  "context": {
    "cookiecutter": {
      "project_name": "Merge Sort",
      "project_slug": "merge_sort",
      "os": "ubuntu",
      "_template": "git@github.com:jun-harashima/cookiecutter-mypackage.git"
    }
  },
  "directory": null
}
```

これで、テンプレートの変更に追従できます。そのためのコマンドが `cruft check` です。実行してみましょう。追従できていれば、以下のように出力されるはずです。

```Shell
$ cruft check
SUCCESS: Good work! Project's cruft is up to date and as clean as possible :).
```

では、テンプレートを変更してみましょう。以下のように、cookiecutter-mypackage を変更します。

```Shell
$ git diff

diff --git a/{{ cookiecutter.project_slug }}/Dockerfile b/{{ cookiecutter.project_slug }}/Dockerfile
index c00b27a..349e493 100644
--- a/{{ cookiecutter.project_slug }}/Dockerfile
+++ b/{{ cookiecutter.project_slug }}/Dockerfile
@@ -1,7 +1,7 @@
 {% if cookiecutter.os == 'rockylinux' -%}
-FROM rockylinux:8.9
+FROM rockylinux:9.3
 {%- else -%}
-FROM ubuntu:22.04
+FROM ubuntu:24.04
 {%- endif %}

 CMD ["/bin/bash"]
```

そして、この変更をレポジトリにプッシュします。

再度、`cruft check` コマンドを実行してみましょう。追従できていないので、以下のように出力されるはずです。

```Shell
$ cruft check
FAILURE: Project's cruft is out of date! Run `cruft update` to clean this mess up.
```

`cruft update` コマンドを実行すれば、テンプレートに追従できます。実行してみましょう。

```Shell
$ cruft update
Respond with "s" to intentionally skip the update while marking your project as up-to-date or respond with "v" to view the changes that will be applied.
Apply diff and update? (y, n, s, v) [y]: y
```

ここでは変更を適用し、プロジェクトを更新したいので、`y` を入力しましょう。以下のようなメッセージが表示されるはずです。

```Shell
Good work! Project's cruft has been updated and is as clean as possible!
```

Dockerfile の中身を確認してみましょう。以下のように、バージョンが変更されているはずです。

```Dockerfile
FROM ubuntu:24.04

CMD ["/bin/bash"]
```

このように、`cruft link` コマンドを実行すれば、生成済みのプロジェクトと生成元のテンプレートをリンクできます。もし生成時にプロジェクトとテンプレートをリンクしたければ、`cruft create` というコマンドが利用できます。


```Shell
$ cruft create https://github.com/jun-harashima/cookiecutter-mypackage
```

この場合、最初から .cruft.json がプロジェクトに追加されます。

最後に、常にテンプレートの変更に追従したければ、CI を設定しておきましょう。cruft の利用方法については M3 の技術ブログ [^2] が参考になります。

これで、このハンズオンは終了です。お疲れ様でした。

[^1]: https://github.com/cruft/cruft/
[^2]: https://www.m3tech.blog/entry/cruft
