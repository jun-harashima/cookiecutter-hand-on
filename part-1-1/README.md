# Part 1-1. テンプレートの利用

cookiecutter [^1] はテンプレートからプロジェクトを生成してくれるツールです。利用事例としては、例えば、以下があります。

- Cookiecutter Template for Data Scientists Working in Docker Containers（Cookpad）[^2]
- Kubernetes 上での Go によるシンプルな API の開発と、その効率化のための cookiecutter template を作った話（M3）[^3]
- 【Tech の道も一歩から】第 46 回「Cookiecutter で Python プロジェクトを高速に開始する」（Sansan）[^4]

なにはともあれ、まずはインストールしてみましょう。公式ドキュメントでは pipx が推奨されていますが、そこはご自身の好みや環境に合わせてください。Mac の場合、brew でもインストールできるようです。

```Shell
$ pipx install cookiecutter
```

インストールしたら、`cookiecutter` コマンドが利用できるはずです。では、早速、テンプレートを利用してみましょう。ここでは、Python パッケージのテンプレートである cookiecutter-pypackage [^5] を利用してみましょう。

```Shell
$ cookiecutter https://github.com/audreyfeldroy/cookiecutter-pypackage.git
  [1/14] full_name (Audrey Roy Greenfeld):
```

`cookiecutter` コマンドを実行すると、氏名を入力するプロンプトが表示されます。ご自身のお名前（e.g. Jun Harashima）を入力してください。

```Shell
  [2/14] email (audreyr@example.com):
```

すると、次はメールアドレスを入力するプロンプトが表示されます。このように、プロジェクトを生成するのに必要な情報を順番に入力していきます。

```Shell
  [4/14] project_name (Python Boilerplate):
```

4 番目に、プロジェクト名を入力するプロンプトが表示されます。ハンズオンなので、なんでもよいです。ここではバブルソートを実装するプロジェクトを想定し、Bubble Sort としてみます。

```Shell
  [5/14] project_slug (bubble_sort):
```

すると、プロジェクト名の略称を入力するプロンプトが表示されます。デフォルトの略称（bubble_sort）が名前（Bubble Sort）から生成されるので、このハンズオンではなにも入力せず、スキップ（そのままエンターキーを入力）してみましょう。

すべての情報を入力（あるいは、スキップ）すると、bubble_sort というディレクトリが生成されているはずです。ディレクトリ内を確認しましょう。

```Shell
$ cd bubble_sort/

$ ls
AUTHORS.rst             CONTRIBUTING.rst        LICENSE                 Makefile                docs/                   requirements_dev.txt    src/                    tox.ini
CODE_OF_CONDUCT.rst     HISTORY.rst             MANIFEST.in             README.rst              pyproject.toml          ruff.toml               tests/
```

cookiecutter は、あなたが先程入力した情報をテンプレート（今回の場合は cookiecutter-pypackage）に注入し、これらのファイルやディレクトリを生成しています。例えば、あなたが入力した氏名を検索してみましょう。

```shell
$ grep -r "Jun Harashima" .
./LICENSE:Copyright (c) 2024, Jun Harashima
./pyproject.toml:  {name = "Jun Harashima", email = "j.harashima@gmail.com"}
./pyproject.toml:  {name = "Jun Harashima", email = "j.harashima@gmail.com"}
./docs/conf.py:copyright = "2024, Jun Harashima"
./docs/conf.py:author = "Jun Harashima"
./docs/conf.py:     'Jun Harashima', 'manual'),
./AUTHORS.rst:* Jun Harashima <j.harashima@gmail.com>
./src/bubble_sort/__init__.py:__author__ = """Jun Harashima"""
```

LICENSE や pyproject.toml 等のファイルにあなたの氏名が注入されているはずです。このように、cookiecutter を利用すれば、任意のテンプレートから自分のプロジェクトを簡単に生成できます。

GitHub を cookiecutter で検索すると、たくさんのテンプレートがヒットします。普段の開発でも活用してみましょう。

次は [Part 1-2. テンプレートの作成](../part-1-2)です。

[^1]: https://github.com/cookiecutter/cookiecutter
[^2]: https://speakerdeck.com/takahiko03/cookiecutter-template-for-data-scientists-working-in-docker-containers
[^3]: https://www.m3tech.blog/entry/kubernetes-api-project-template
[^4]: https://buildersbox.corp-sansan.com/entry/2022/04/05/110000
[^5]: https://github.com/audreyfeldroy/cookiecutter-pypackage
