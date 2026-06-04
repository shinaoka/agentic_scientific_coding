# Handoff: B4/M1向け「Scientific Agentic Coding」基礎教材設計

## 目的

B4/M1 学生向けに、AI agentic coding 時代の科学計算教育教材を設計する。

ただし、最初から AI にコードを書かせる教材ではない。まず学生が手書きで基本概念を理解し、その上で AI や web search を使って自分で調べ、コードや説明を検証できるようにする。

教材本体は網羅的な教科書ではなく、学ぶべき概念の地図と、手書き演習、注意事項を与えるものとする。

## 基本方針

- B4/M1 を主対象とする。
- 出発点として、Python で `for` loop, `if`, 関数, list/array 程度を書いた経験は仮定してよい。
- ただし、software engineering, testing, package structure, abstraction, reproducibility, Git, static typing, ownership などはほぼ未習得と考える。
- 最初は手書きで基本概念を学ぶ。
- 各章では概要のみを説明する。
- 詳細な文法や実装例は、学生が AI や web search を使って調べる。
- 章末に手書き問題を置く。
- 「学生への注意事項」を置く。
- AI 採点用の指令は教材には書かない。
- 採点時には、Exercise と Notes for students に基づいて AI に評価させればよい。

## 重要な設計判断

### AI 採点指令は教材から外す

当初は、各章末に「AI への採点指令」を置く案があった。

しかし、これは外す。

理由:

- 学生への注意事項と採点基準は一致していた方がよい。
- AI 採点指令を教材内に明示すると、学生が採点基準をなぞるだけになりやすい。
- 教材としては自然でない。
- 採点時には「以下の Exercise と Notes for students に基づいて評価せよ」と AI に渡せばよい。

したがって、各章の基本構成は以下とする。

```text
Chapter X: Concept name

Goal:
この章で理解すべきことを 2〜3 行で説明。

Minimal explanation:
概念の概要だけを簡潔に説明。

Things to look up:
AI や web search で調べるべきキーワードを列挙。

Hand-written exercise:
紙に書いて解く問題。

Notes for students:
解答時に注意すべき点。採点もこの観点に基づく。

Optional coding exercise:
必要なら実際に Python で確認する課題。
```

## 教育上の中心思想

AI はコードもテストも書ける。したがって、学生に必要なのは「コードを書く能力」だけではない。

重要なのは以下。

- 仕様を言語化できる。
- 入力・出力・副作用を区別できる。
- コードがどの数式・アルゴリズムに対応するか説明できる。
- 計算量を見積もれる。
- 境界条件を考えられる。
- failure mode を考えられる。
- テストすべき性質を設計できる。
- AI が書いたコードや説明が十分か評価できる。
- 実行結果を再現可能な形で保存できる。
- diff を読んで、AI agent が何を変えたか確認できる。

## 階層構造

教材は階層化する。

### Level 0: 調べ方・読み方

目的: AI や web search を使って自学するための基本姿勢を作る。

扱う概念:

- AI に質問する
- web search する
- 公式ドキュメントを読む
- コードを読む
- 分からない点を言語化する
- AI の説明を鵜呑みにしない
- 「正しいですか？」ではなく、具体的な観点で確認する

例:

- 「この関数は何を計算していますか？」
- 「入力と出力を整理してください」
- 「境界条件で壊れませんか？」
- 「計算量を見積もってください」
- 「この説明の前提を列挙してください」

### Level 1: 基本文法・制御

目的: 小さなプログラムを読み、状態変化を追えるようにする。

扱う概念:

- 変数
- 代入
- 式
- 条件分岐
- ループ
- 関数
- 配列・リスト
- 辞書・集合
- 入出力

注意:

- 代入は数学の等号ではない。
- ループでは、何が更新されるかを追う。
- 関数では、入力、出力、副作用を区別する。
- 空配列、要素数 1、境界値を考える。

### Level 2: プログラムの構造

目的: 大きくなり始めたコードを分割し、意味のある単位に整理できるようにする。

扱う概念:

- 関数分割
- abstraction
- interface
- module
- mutable / immutable
- side effect
- error handling

注意:

- 何を public にし、何を内部実装にするかを考える。
- 実装詳細と interface を区別する。
- 変更されるデータと変更されないデータを区別する。
- 副作用がある関数と、値を返すだけの関数を区別する。

### Level 3: 正しさ・検証

目的: 「動いた」ではなく、「なぜ信用できるか」を説明できるようにする。

扱う概念:

- 仕様
- edge case
- unit test
- regression test
- reference implementation
- TDD
- property-based thinking
- reproducibility の基礎

注意:

- テストは実装の写しではなく、仕様の確認である。
- trivial case だけでは不十分。
- 失敗例を考える。
- 既知解、保存則、対称性、極限、収束性を使う。
- AI がテストを書いても、そのテストが十分かは人間が判断する。

### Level 4: アルゴリズム

目的: 基本アルゴリズムを、実装ではなく、計算量・正しさ・誤差・failure mode の観点から理解する。

扱う概念:

- 探索
- ソート
- 計算量
- 数値積分
- 方程式解法
- 線形代数
- Monte Carlo
- 誤差
- 収束
- 安定性

注意:

- sort が `O(N^2)` になっていないか考える。
- binary search では、整列済みという前提を明記する。
- numerical integration では、1回の値が合うだけでは不十分。
- 刻み幅依存性を見る。
- ODE や反復法では、停止条件と失敗条件を考える。
- Monte Carlo では乱数 seed と誤差評価を考える。

### Level 5: Scientific workflow

目的: 科学計算を再現可能な project として管理できるようにする。

この Level の最初に Git を置く。

#### 5.1 Git basics

Git は、単なる開発ツールではなく、科学計算 project の履歴を残すための基盤として導入する。

扱う概念:

- repository
- commit
- diff
- history
- AI agent が変更した diff を読む

詳細な Git コマンド一覧は不要。

注意:

- repository は履歴付き project folder である。
- commit は作業の節目を保存すること。
- diff は何を変えたかを見るためのもの。
- AI agent が変更したら diff を読む。
- 何を変更したか説明できるようにする。
- 大きすぎる変更を一つの commit にまとめない。

#### 5.2 Project structure

扱う概念:

- source code
- data
- results
- scripts
- README
- notes / 作業ログ

注意:

- 何を保存し、何を再生成するかを決める。
- 入力、コード、出力を混ぜない。
- README を project の入口にする。

#### 5.3 Reproducible execution

扱う概念:

- 入力パラメータ
- 出力ファイル
- 乱数 seed
- 実行ログ
- version 情報

注意:

- 結果だけでなく、条件を残す。
- 出力ファイルと入力条件を対応づける。
- 乱数を使う場合は seed を記録する。

#### 5.4 Analysis and reporting

扱う概念:

- plotting
- benchmark
- profiling
- report
- figure generation

注意:

- 計算と plot を分ける。
- 図を再生成できるようにする。
- benchmark では測定条件を残す。
- profiling は「遅い場所」を特定するために使う。

#### 5.5 AI-agent workflow

扱う概念:

- 作業ログ
- 変更理由
- diff review
- テスト・検証結果
- 計算結果とコード変更の対応

注意:

- AI agent に作業ログを書かせる。
- AI agent が何を変えたか diff で確認する。
- 変更理由を notes や README に残す。
- 計算結果とコード変更を対応づける。
- 「動いた」だけで終わらせない。

### Level 6: Rust / agentic coding への移行

目的: Python で身につけた code reading / validation / workflow を、Rust と AI agentic coding に移す。

扱う概念:

- Rust の所有権
- borrowing
- mutability
- 型
- `Result` / `Option`
- Cargo
- `cargo test`
- examples を改造する
- AI agent と一緒に安全に変更する

注意:

- 最初から低レベル Rust infrastructure を書かせない。
- examples から始める。
- パラメータを変える。
- 高レベルコードを少し変更する。
- テストを追加する。
- reference result と比較する。
- 所有権・可変性・data flow が明示的であることを利用する。

## 章案

### Chapter 0: How to learn with AI and web search

Goal:
AI や web search を使って調べる方法を学ぶ。

Minimal explanation:
教材はすべてを説明しない。必要な知識は、自分で調べ、比較し、確認する。

Things to look up:

- Python official documentation
- Stack Overflow の読み方
- how to ask coding questions
- search query examples
- hallucination in AI coding

Hand-written exercise:

- ある関数の説明を AI に求めるための質問を 5 個書く。
- 「正しいですか？」ではなく、具体的な確認観点を含める。

Notes for students:

- AI の答えをそのまま信用しない。
- 入力、出力、境界条件、計算量、検証方法を聞く。
- 分からない単語をそのままにしない。

### Chapter 1: Variables and state

Goal:
変数、代入、状態変化を理解する。

Hand-written exercise:

- `x = x + 1` を、数学の等式ではなく、状態更新として説明する。
- 短いコード片の各行の後で、変数の値がどう変わるか表にする。

Notes for students:

- 代入と等式を混同しない。
- 変数名と値を区別する。
- 更新順序を追う。

### Chapter 2: Conditionals

Goal:
条件分岐と境界条件を理解する。

Hand-written exercise:

- 絶対値関数、符号関数、区分的関数を擬似コードで書く。
- `x = 0` の扱いを明示する。

Notes for students:

- 条件の重なりを確認する。
- どの条件にも入らない場合がないか確認する。
- 境界値を必ず考える。

### Chapter 3: Loops

Goal:
ループと accumulator を理解する。

Hand-written exercise:

- 配列の和、最大値、平均を求める擬似コードを書く。
- 各ループで何が更新されるか説明する。

Notes for students:

- 初期値を明示する。
- 空配列を考える。
- off-by-one error に注意する。
- loop invariant を言葉で書く。

### Chapter 4: Functions

Goal:
関数の入力、出力、副作用を理解する。

Hand-written exercise:

- 平均と分散を計算する関数の仕様を書く。
- 入力、出力、例外的ケースを列挙する。

Notes for students:

- 引数を変更するのか、新しい値を返すのか区別する。
- 関数名は機能を表す。
- 一つの関数に複数の責務を詰め込まない。

### Chapter 5: Data structures

Goal:
データ構造を目的に応じて選ぶ。

Hand-written exercise:

- 学生名と点数の表を管理する方法を考える。
- list, dictionary, set のどれを使うべきか説明する。

Notes for students:

- 順序が必要か考える。
- 重複を許すか考える。
- 高速な検索が必要か考える。

### Chapter 6: Abstraction and interface

Goal:
実装詳細と interface を区別する。

Hand-written exercise:

- ベクトルを list として扱う場合と、Vector 型として扱う場合の違いを書く。
- public に見せる操作を列挙する。

Notes for students:

- 何を隠すかを考える。
- 何を保証するかを考える。
- 使う側が知るべき情報を最小化する。

### Chapter 7: Testing and TDD

Goal:
期待する振る舞いを先に言語化する。

Hand-written exercise:

- 平均を返す関数に対して、テストケースを 5 個設計する。
- 各テストが何を確認しているか書く。

Notes for students:

- trivial case だけにしない。
- 空入力、要素 1 個、同じ値、負値、大きい値を含める。
- テストは実装の写しではなく、仕様の確認である。

### Chapter 8: Complexity

Goal:
計算量を見積もる。

Hand-written exercise:

- 一重ループ、二重ループ、三重ループの計算量を見積もる。
- `N = 10^3, 10^6` で何が起こるか考える。

Notes for students:

- 小さい入力で速いことと、漸近的に速いことを混同しない。
- loop の回数を数える。
- hidden constant にも注意するが、まずは次数を見る。

### Chapter 9: Search and sort

Goal:
探索とソートの基本を理解する。

Hand-written exercise:

- linear search と binary search の違いを書く。
- 小さい配列に対して selection sort または insertion sort を手で実行する。
- merge sort の考え方を図で説明する。

Notes for students:

- binary search では整列済みという前提が必要。
- sort が `O(N^2)` になっていないか考える。
- 重複要素をどう扱うか考える。
- stable sort が必要か考える。

### Chapter 10: Numerical integration

Goal:
数値積分と誤差・収束を理解する。

Hand-written exercise:

- `∫_0^1 x^2 dx` を台形則で近似する手順を書く。
- 刻み幅を半分にしたとき、誤差がどう変わるべきか考える。

Notes for students:

- 既知解と比較する。
- 1回の値が合うだけでは不十分。
- 刻み幅依存性を見る。
- 非滑らかな関数での失敗を考える。

### Chapter 11: Root finding

Goal:
二分法と Newton 法の考え方を理解する。

Hand-written exercise:

- 二分法の不変条件を書く。
- Newton 法が失敗する例を考える。

Notes for students:

- 二分法では解を挟む条件が必要。
- 停止条件を明示する。
- Newton 法は初期値に依存する。
- 微分が小さい場合や複数解の場合に注意する。

### Chapter 12: Linear algebra computations

Goal:
行列・ベクトル計算の shape と計算量を理解する。

Hand-written exercise:

- matrix-vector product と matrix-matrix product の擬似コードを書く。
- 計算量を見積もる。

Notes for students:

- shape mismatch に注意する。
- index の順序を確認する。
- 行列積は通常 `O(N^3)` である。
- 対称性や疎性がある場合は利用できるか考える。

### Chapter 13: Monte Carlo and randomness

Goal:
乱数を使う計算の再現性と誤差を理解する。

Hand-written exercise:

- Monte Carlo で円周率を推定する方法を書く。
- seed とサンプル数をどう記録するか説明する。

Notes for students:

- seed を記録する。
- サンプル数依存性を見る。
- 誤差棒を考える。
- 1回の結果だけで判断しない。

### Chapter 14: Git basics for scientific workflow

Goal:
Git を科学計算 project の履歴管理として理解する。

Hand-written exercise:

- repository, commit, diff を自分の言葉で説明する。
- AI agent がコードを変更した後、何を確認すべきか列挙する。

Notes for students:

- commit は作業の節目を保存すること。
- diff は変更内容を確認するためのもの。
- AI agent が変更した diff を読む。
- 大きすぎる変更を一つの commit にまとめない。
- なぜ変更したか説明できるようにする。

### Chapter 15: Project structure and reproducibility

Goal:
計算 project を再現可能に整理する。

Hand-written exercise:

- `src/`, `data/`, `results/`, `scripts/`, `README` を含む project structure を設計する。
- ある計算結果を再現するために必要な情報を列挙する。

Notes for students:

- 入力、コード、出力を混ぜない。
- 結果ファイルと入力条件を対応づける。
- README を project の入口にする。
- 乱数 seed, parameter, version を残す。

### Chapter 16: AI-agent workflow

Goal:
AI agent を使うときの基本 workflow を理解する。

Hand-written exercise:

- AI agent にコードを変更させた後、人間が確認すべき項目を 10 個書く。
- 「正しいですか？」ではなく、具体的な review prompt を書く。

Notes for students:

- AI agent の説明を鵜呑みにしない。
- diff を読む。
- テストと validation を確認する。
- 計算結果とコード変更を対応づける。
- 作業ログを残す。

### Chapter 17: Transition to Rust

Goal:
Python で学んだ読み方・検証方法を Rust に移す。

Hand-written exercise:

- Rust の `mut`, ownership, borrowing が、Python の variable update とどう違うか調べて説明する。
- `cargo test` が何をするか説明する。

Notes for students:

- Rust を最初から全部理解しようとしない。
- examples から始める。
- 所有権、可変性、data flow が明示的であることに注目する。
- AI agent に説明させた内容を、コンパイルとテストで確認する。

## 既存教材から参考にするもの

別エージェントは、以下の有名教材を参照してよい。

- Harvard CS50
  - 入門 CS
  - abstraction, algorithms, data structures, software engineering の導入に有用

- MIT 6.0001 / 6.100L
  - Python による計算機科学入門
  - 小さいプログラム、条件分岐、ループ、関数、アルゴリズム導入に有用

- Software Carpentry
  - 研究者向け実践教材
  - Python, shell, Git, reproducible workflow に有用

- Berkeley CS61A
  - abstraction を中心にした入門
  - 関数、抽象化、プログラム構造に有用

- Think Python
  - Python 入門
  - 手書き演習に落とし込みやすい

- The Turing Way
  - reproducible research, project organization, version control, collaboration に有用

ただし、この教材は既存教材の縮約ではない。AI agentic coding 時代向けに、概念地図、手書き演習、注意事項に再構成する。

## Markus との議論からの背景

この教材設計は、Tensor4all.jl を maintenance mode に置き、tensor4all-rs / tenferro-rs と reliable scientific agentic coding の教育 workflow に注力する議論から派生した。

重要な論点:

- Rust + AI agents で B4/M1 や Ph.D. student が本当に理解しながら開発できるかは未確立。
- しかし、未確立だから待つのではなく、教育 material と workflow を自分たちで作るべき。
- Julia/Python の表面構文は pseudocode に近いが、大規模 scientific library の理解が簡単になるとは限らない。
- Rust は verbose だが、ownership, mutation, data flow, compiler feedback, Cargo により、software invariants を追いやすい。
- 初学者にいきなり low-level Rust infrastructure を書かせる必要はない。
- Python で code reading, validation, testing, reproducibility を学び、その workflow を Rust に移す。

## 次にやるべき作業

別エージェントには、以下を依頼するとよい。

1. 上記の Level 構造をもとに、教材目次を正式化する。
2. 各章について、1ページ程度の skeleton を作る。
3. 各章に以下を含める。
   - Goal
   - Minimal explanation
   - Things to look up
   - Hand-written exercise
   - Notes for students
   - Optional coding exercise
4. まず Chapter 0–5 を試作する。
5. B4/M1 が 1回 60–90 分で消化できる粒度にする。
6. 説明は短くし、詳細は AI/web search で調べさせる。
7. Notes for students は、そのまま採点観点になるように書く。
8. AI 採点用指令は教材本文には入れない。

## 注意すべきトーン

- 長い講義ノートにしない。
- 網羅的に説明しすぎない。
- 学生に「何を調べるべきか」を示す。
- 手書きで考える課題を重視する。
- AI は補助として使うが、判断責任は学生に置く。
- Rust を押しつけるのではなく、Python で身につけた workflow の次段階として位置づける。
