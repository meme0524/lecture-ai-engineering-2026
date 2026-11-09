# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：
- `--trace` で見えたツール呼び出し（ツール名と引数）：
    - [tool call]   search_glossary({'query': 'LoRA'})
    - [tool call]   search_glossary({'query': 'QLoRA'})
    - [tool return] search_glossary -> [{"term": "LoRA", "lecture": 2, "definition": "事前学習済みの重みは固定したまま、重みの更新分を2つの小さな行列の積（低ランク行列）で表し、その小さな行...}] 
- 1つの質問での LLM 呼び出し回数（`requests`）：2
- 追加したツールと、docstring に書いた説明：[make_quiz]  指定した回の用語から１つを選び、確認問題と答えを返す。「第2回の確認問題を出して」のように、復習用の問題を求められたときに使う。引数: {"lecture": {"type": "integer"}}

## 宿題

- 取り組んだレベル：実装
- やったこと：
    - `app/tools.py` に `save_learning_log(topic, note)` を作成した。
    - `learning_log/` フォルダに、日時をファイル名にしたJSON形式で学習記録を保存するようにした。
    - `app/agent.py` で `save_learning_log` を `requires_approval=True` 付きで登録した。
    - `DeferredToolRequests` を `output_type` に追加し、承認待ちのツール呼び出しを扱えるようにした。
    - CLIで保存前に `[y/N]` を表示し、承認結果を渡してエージェントを再開する処理を追加した。
- 結果：
    - 承認前はツールが実行されず、`DeferredToolRequests` が返ることを確認した。
    - 承認すると処理が再開され、`learning_log/YYYYMMDD_HHMMSS.json` が作成される。
    - 承認しない場合は保存されない。

- 気づき・疑問：
    - `requires_approval=True` によって、ファイル保存のような取り消しにくい処理を実行前に確認できる。
    - 承認後の再実行には、元の `message_history` と `DeferredToolResults` が必要になる。
    - ツールを登録しただけでは必ず呼ばれるとは限らないため、エージェントの指示と依頼文を具体的にする必要がある。
        - 例えば「これまでの学習記録を保存してください」では、具体的なトピックと内容を書くように誘導する（ここの返答については第２回でのファインチューニングでより分かりやすく誘導できるかもしれない）。
        - トピックは「○○」、メモは「○○」のように明示することで、正確に引数を与えることができる。