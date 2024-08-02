# Part 2-1. フックの利用

フック [^1] を利用すれば、プロジェクト生成時にスクリプトを実行できます。フックには以下の 3 種類があります。

- pre_prompt ... プロンプト表示前
- pre_gen_project ... プロンプト表示後、プロジェクト生成前
- post_gen_project ... プロジェクト生成後

例えば、プロジェクト生成後、そのままプロジェクトのバージョンを管理してみましょう。この場合、post_gen_project を利用します。

今回の場合、post_gen_project.sh という名前のスクリプトを hooks という名前のディレクトリに配置します。現時点で cookiecutter-mypackage の中身は以下の通りです。

```Text
cookiecutter-mypackage/
├── README.md
├── cookiecutter.json
├── hooks
│   └── post_gen_project.sh
└── {{ cookiecutter.project_slug }}
    ├── Dockerfile
    └── README.md
```

なお、フックには Python スクリプトも利用できます。クロスプラットフォームの互換性を保持したい場合、Python スクリプトが推奨されています。

さて、プロジェクトのバージョンを管理するため、post_gen_project.sh に以下を記載します。GitHub Enterprise 等を利用している場合、`github.com` の部分は適切なドメイン名に変更してください。

```Shell
#!/bin/bash

git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:{{ cookiecutter.organization_name }}/{{ cookiecutter.project_slug }}.git
git push -u origin main
```

organization 名が必要になるため、以下のように、`organization_name` という変数を cookiecutter.json に追加しましょう。

```JSON
{
    "project_name": "My Project",
    "project_slug": "{{ cookiecutter.project_name.lower().replace(' ', '_').replace('-', '_') }}",
    "organization_name": "",
    "os": ["rockylinux", "ubuntu"]
}
```

では、`cookiecutter` コマンドを実行してみましょう。ただし、あらかじめ GitHub にリモートレポジトリを作成しておく必要があります。

```Shell
  [3/14] organization_name:
```

3 番目に、organization 名を入力するプロンプトが表示されます。リモートレポジトリを作成した organization 名を入力してください。

最後まで入力すると、プロジェクトを生成した後、フックが発火し、リモートレポジトリに push されるはずです。GitHub を確認してみてください。

次は [Part 2-2. テンプレートの追従](../part-2-2)です。

[^1]: https://cookiecutter.readthedocs.io/en/stable/advanced/hooks.html
