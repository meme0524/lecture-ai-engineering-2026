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

- 取り組んだレベル：基礎 / 実装 / 発展
- やったこと：
- 結果：
- 気づき・疑問：
