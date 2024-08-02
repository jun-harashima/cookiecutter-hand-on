# Part 1-2. テンプレートの作成

Part 1-1 では既存のテンプレートを利用しました。しかし、自分の開発スタイルに合ったテンプレートが常に存在するとは限りません。そこで、Part 1-2 では自分のテンプレートを作成してみましょう。

テンプレート名には cookiecutter- というプリフィックスをつけるのが一般的です。このハンズオンでは、cookiecutter-pypackage をもじって、cookiecutter-mypackage というテンプレートを作成してみましょう。まず、cokkiecutter-mypackage というディレクトリを作成します。

```Shell
$ mkdir cookiecutter-mypackage
```

最低限必要なファイルとディレクトリは以下の通りです。

```Text
cookiecutter-mypackage/
├── cookiecutter.json
└── {{ cookiecutter.project_slug }}
```

さっそく作成してみましょう。

```Shell
$ cd cookiecutter-mypackage/

$ touch cookiecutter.json

$ mkdir "{{ cookiecutter.project_slug }}"
```

cookiecutter.json に、プロジェクト生成時に表示されるプロンプト（とデフォルト値）を記載します。ここでは以下のように記載してください。

```JSON
{
    "project_name": "project_name",
    "project_slug": "{{ cookiecutter.project_name.lower().replace(' ', '_').replace('-', '_') }}"
}
```

{{ cookiecutter.project_slug }} がテンプレートです。別の言い方をすれば、プロジェクトを生成する際、{{ cookiecutter.project_slug }} 内のファイルやディレクトリが展開されます。ひとまず、以下のような README.md を追加してみましょう。

```Shell
$ cd \{\{ cookiecutter.project_slug \}\}/

$ echo "# {{ cookiecutter.project_name }}" > README.md
```

現時点で cookiecutter-mypackage の中身は以下の通りです。

```Text
cookiecutter-mypackage/
├── cookiecutter.json
└── {{ cookiecutter.project_slug }}
    └── README.md
```

それでは、テンプレートからプロジェクトを生成してみましょう。cookiecutter-mypackage が存在するディレクトリに移動した後、以下のように `cookiecutter` コマンドを実行してみてください。

```Shell
$ cookiecutter cookiecutter-mypackage/
  [1/2] project_name (My Package):
```

なお、Part 1-1 では `cookiecutter` コマンドの引数として GitHub の URL を使用しました。テンプレートが手元にあれば、URL でなく、パスでも問題ありません。

特に問題がなければ、プロジェクト名を入力するプロンプトが表示されたはずです。Part 1-1 と同じく、なんでもよいです。ここでは Merge Sort としてみます。

```Shell
  [2/2] project_slug (merge_sort):
```

すると、プロジェクト名の略称を入力するプロンプトが表示されます。Part 1-1 と同じく、スキップしましょう。

これで、`merge_sort` というディレクトリが生成されているはずです。ディレクトリ内を確認してみましょう。

```Shell
$ cd merge_sort/

$ ls
README.md
```

README.md の内容を確認してみましょう。

```Shell
$ cat README.md
# Merge Sort
```

テンプレートの README.md では `# {{ cookiecutter.project_name }}` という文字列でした。cookiecutter では Jinja [^1] を利用し、テンプレート内の変数（e.g. `cookiecutter.project_name`、`cookiecutter.project_slug`）を値（e.g. `Merge Sort`、`merge_sort`）に展開しています。

Jinja では制御文も記述できます。例えば、以下のような Dockerfile をテンプレートに追加してみましょう。README.md と同様、{{ cookiecutter.project_slug }} というディレクトリに追加します。

```Dockerfile
{% if cookiecutter.os == 'rockylinux' -%}
FROM rockylinux:8.9
{%- else -%}
FROM ubuntu:22.04
{%- endif %}

CMD ["/bin/bash"]
```

そして、以下のように、`os` という変数を cookiecutter.json に追加します。`project_slug` の行末に `,` を入れるのを忘れないでください。

```JSON
{
    "project_name": "My Project",
    "project_slug": "{{ cookiecutter.project_name.lower().replace(' ', '_').replace('-', '_') }}",
    "os": ["rockylinux", "ubuntu"]
}
```

選択変数 [^2] を利用すれば、プロジェクト生成時に選択肢を提示してくれます。cookiecutter-mypackage が存在するディレクトリに移動した後、以下のように `cookiecutter` コマンドを実行してください。

```Shell
$ cookiecutter -f cookiecutter-mypackage/
  [1/3] project_name (My Package):
```

今回は `-f` オプションを利用しました。プロジェクト生成時に同名のディレクトリがあった場合、内容を上書きしてくれます。先程と同様、Merge Sort としてみます。

```Shell
  [2/3] project_slug (merge_sort):
```

ここも、先程と同様、スキップしましょう。

```Shell
  [3/3] Select os
    1 - rockylinux
    2 - ubuntu
    Choose from [1/2] (1):
```

すると、先程と違い、OS を選択するプロンプトが表示されます。例えば、`2` を入力してみましょう。ちなみに、`1` と `2` 以外を入力すると、`Please select one of the available options` というエラーが表示されます。

生成されたプロジェクトを確認してみましょう。

```Shell
$ cd merge_sort/

$ ls
Dockerfile	README.md
```

Dockerfile の中身を確認してみましょう。以下のようになっていれば、成功です。

```Dockerfile
FROM ubuntu:22.04

CMD ["/bin/bash"]
```

このようにファイルやディレクトリを追加していくことで、自分のテンプレートが作成できます。あとは、任意のタイミングで、GitHub でテンプレートを管理しておくとよいです。

次は [Part 2-1. フックの利用](../part-2-1)です。

[^1]: https://jinja.palletsprojects.com/en/3.1.x/
[^2]: https://cookiecutter.readthedocs.io/en/stable/advanced/choice_variables.html
