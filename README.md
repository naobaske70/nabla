<!doctype html>
<html lang="ja">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>ナブラ演算子ゲーム（一人用）</title>

<style>
:root {
  --gold: #ebcf88;
  --cream: #fff9eb;
}

* { box-sizing: border-box; }

body {
  margin: 0;
  padding: 12px;
  color: var(--cream);
  background: radial-gradient(circle at top, #37644e, #142b24 78%);
  font-family: -apple-system, BlinkMacSystemFont,
    "Hiragino Kaku Gothic ProN", "Yu Gothic", sans-serif;
}

main {
  width: min(100%, 900px);
  margin: auto;
}

h1 {
  margin: 8px 0;
  color: var(--gold);
  text-align: center;
  font-size: clamp(23px, 4vw, 34px);
}

h2 {
  margin: 0 0 9px;
  color: var(--gold);
  font-size: 18px;
}

button, select { font: inherit; }
button { cursor: pointer; touch-action: manipulation; }
button:disabled { cursor: default; opacity: .55; }

button:focus-visible,
select:focus-visible,
input:focus-visible {
  outline: 3px solid white;
  outline-offset: 3px;
}

[hidden] { display: none !important; }

.intro, .hint {
  font-size: 13px;
  line-height: 1.5;
}

.intro { text-align: center; }
.hint { color: #d7e6db; }

.panel {
  margin: 10px 0;
  padding: 12px;
  border: 1px solid #91aa99;
  border-radius: 14px;
  background: #102a22de;
}

.topbar, .actions {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  gap: 9px;
}

.topbar {
  padding: 9px;
  border: 1px solid #91aa99;
  border-radius: 12px;
  background: #10291fdd;
}

.counter {
  min-width: 105px;
  padding: 5px 9px;
  border-radius: 8px;
  background: #ffffff18;
  text-align: center;
  font-size: 12px;
}

.counter strong {
  display: block;
  color: var(--gold);
  font-size: 18px;
}

.control {
  min-height: 42px;
  padding: 7px 12px;
  border: 1px solid var(--gold);
  border-radius: 9px;
  background: #173c30;
  color: #fff4d2;
}

.actions { margin: 10px 0; }

.field {
  min-height: 215px;
  margin: 11px 0;
  padding: 14px;
  border: 2px solid #a6c8ac;
  border-radius: 16px;
  background: #244b3d;
  box-shadow: inset 0 2px 10px #0004, 0 5px 15px #0003;
}

.field.enemy {
  border-color: #d79a85;
  background: #4d3937;
}

.enemy .control { background: #58382e; }

.field-head {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  align-items: center;
  gap: 8px;
  margin-bottom: 14px;
}

.dimension {
  margin-left: 8px;
  padding: 3px 9px;
  border: 1px solid #ffffff88;
  border-radius: 99px;
  font-size: 13px;
}

.cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: stretch;
  gap: 10px;
}

.playing-card {
  display: grid;
  grid-template-rows: 20px minmax(0,1fr) 20px;
  align-items: center;
  justify-items: center;
  flex: 0 0 auto;
  width: 107px;
  min-height: 142px;
  padding: 7px;
  border: 3px solid #bc9a64;
  border-radius: 12px;
  background: #fffaf0;
  color: #253042;
  box-shadow: 0 5px 0 #7a6246, 0 8px 12px #0005;
  text-align: center;
}

.field .playing-card {
  width: 133px;
  min-height: 168px;
}

.enemy-card {
  border-color: #c77e73;
  background: #fff0e8;
}

.mine-card {
  border-color: #87ac8e;
  background: #f5fff0;
}

.hand-card {
  border-color: #cda957;
  background: #fff8e4;
}

.hand-card.selected {
  outline: 3px solid #ffe58b;
  transform: translateY(-4px);
}

.hand-card.extra {
  outline: 3px solid #8ff0b8;
  transform: translateY(-4px);
}

.card-top, .card-bottom {
  width: 100%;
  font-size: 11px;
  font-weight: bold;
}

.card-top { text-align: left; }
.card-bottom { text-align: right; }

.card-symbol {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  min-width: 0;
  max-height: 110px;
  overflow: auto;
  font-size: 21px;
  text-align: center;
}

.card-symbol mjx-container {
  max-width: 100%;
  margin: 0 auto !important;
}

.field .card-symbol { font-size: 24px; }

.field .card-symbol mjx-container {
  font-size: 90% !important;
}

.limit-card .card-symbol mjx-container {
  font-size: 68% !important;
}

#hand {
  flex-wrap: nowrap;
  justify-content: flex-start;
  overflow-x: auto;
  padding: 7px 4px 12px;
}

#opponentHand {
  min-height: 83px;
  align-items: center;
}

.card-back {
  display: grid;
  place-items: center;
  flex: 0 0 auto;
  width: 52px;
  height: 75px;
  border: 3px solid #ddc484;
  border-radius: 9px;
  background: repeating-linear-gradient(
    45deg, #294b76 0 7px, #193653 7px 14px
  );
  box-shadow: inset 0 0 0 4px #173451, 0 4px 8px #0006;
  color: #ffe6a2;
  font-size: 24px;
  font-weight: bold;
}

.empty {
  display: grid;
  place-items: center;
  width: 100%;
  min-height: 110px;
  border: 2px dashed #ffffff70;
  border-radius: 12px;
}

select {
  min-height: 42px;
  padding: 6px;
  border-radius: 8px;
}

#status, #notice {
  min-height: 45px;
  padding: 10px;
  border-radius: 9px;
  background: #ffffff13;
  color: #ffe3a0;
  white-space: pre-wrap;
}

#log {
  max-height: 350px;
  overflow: auto;
}

.log-entry {
  margin: 8px 0;
  padding: 10px;
  border-left: 3px solid var(--gold);
  border-radius: 5px;
  background: #ffffff10;
  line-height: 1.6;
  overflow-wrap: anywhere;
  white-space: pre-wrap;
}

.changed { animation: appear .65s ease-out; }
.field-flash { animation: flash .65s ease-out; }

@keyframes appear {
  from {
    opacity: .25;
    transform: translateY(12px) scale(.85);
  }
  to {
    opacity: 1;
    transform: none;
  }
}

@keyframes flash {
  45% {
    box-shadow: inset 0 0 30px #ffe58b, 0 0 25px #ffe58b;
  }
}

.fly-card {
  position: fixed;
  z-index: 1000;
  display: grid;
  place-items: center;
  width: 96px;
  height: 128px;
  padding: 8px;
  border: 3px solid var(--gold);
  border-radius: 12px;
  background: #fff8e4;
  color: #253042;
  font-size: 17px;
  font-weight: bold;
  text-align: center;
  pointer-events: none;
  box-shadow: 0 9px 20px #0008;
  transition: transform .62s ease-in, opacity .62s ease-in;
}

.fly-card.enemy-fly {
  background: #fff0e8;
  border-color: #d79a85;
}

.fly-card.go {
  transform: translate(var(--dx),var(--dy))
    rotate(14deg) scale(.55);
  opacity: 0;
}

.overlay {
  position: fixed;
  inset: 0;
  z-index: 1100;
  display: grid;
  place-items: center;
  padding: 15px;
  background: #07150de8;
}

.dialog {
  width: min(100%,560px);
  max-height: 92vh;
  overflow: auto;
  padding: 20px;
  border: 3px solid var(--gold);
  border-radius: 17px;
  background: #173729;
  box-shadow: 0 0 35px #e9c77988;
}

.dialog h2 {
  text-align: center;
  font-size: 27px;
}

.dialog p, .dialog li { line-height: 1.6; }

#menuMessage {
  color: #ffe3a0;
  text-align: center;
  white-space: pre-wrap;
}

#rulesBox {
  margin-top: 16px;
  padding: 4px 12px;
  border: 1px solid #ffffff55;
  border-radius: 10px;
  background: #ffffff10;
}

@media (max-width:600px) {
  .field { min-height: 205px; padding: 10px; }
  .field .playing-card { width: 105px; min-height: 145px; }
  .field .card-symbol { font-size: 19px; }
  .playing-card { width: 89px; min-height: 125px; }
  .card-symbol { font-size: 17px; }
  .card-back { width: 44px; height: 65px; }

  .limit-card .card-symbol mjx-container {
    font-size: 62% !important;
  }
}

@media (prefers-reduced-motion:reduce) {
  *, *::before, *::after {
    animation-duration: .01ms !important;
    transition-duration: .01ms !important;
  }
}
</style>

<script>
window.MathJax = {
  tex: { inlineMath: [["\$", "\$"]] },
  startup: { typeset: false }
};
</script>

<script defer
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
</script>
</head>

<body>
<main>
  <h1>∇ ナブラ演算子ゲーム（一人用）</h1>

  <p class="intro">
    手札を選び、対象の基底または「この場に実行」を押してください。
  </p>

  <div id="counts" class="topbar">
    メニューから開始してください。
  </div>

  <div class="actions">
    <button id="openSettings" class="control"
      aria-expanded="false" aria-controls="settingsPanel">
      状況・設定を開く
    </button>

    <button id="openHistory" class="control"
      aria-expanded="false" aria-controls="historyPanel">
      操作履歴を開く
    </button>

    <button id="openMenu" class="control">
      メニュー
    </button>

    <button id="quickRules" class="control">
      遊び方
    </button>
  </div>

  <div id="notice" class="panel"
    role="status" aria-live="polite">
    起動しています…
  </div>

  <section id="settingsPanel" class="panel" hidden>
    <h2>状況・設定</h2>

    <div id="status">起動しています…</div>

    <div class="actions">
      <label for="difficulty">相手の強さ</label>

      <select id="difficulty">
        <option value="easy">初級</option>
        <option value="normal" selected>中級</option>
        <option value="hard">上級</option>
        <option value="expert">超級</option>
      </select>

      <label for="reverseDivide">
        <input id="reverseDivide" type="checkbox">
        関数÷基底
      </label>

      <button id="cancel" class="control" disabled>
        選択解除
      </button>

      <button id="reset" class="control" disabled>
        この難易度で最初から
      </button>
    </div>
  </section>

  <section id="historyPanel" class="panel" hidden>
    <h2>操作履歴</h2>
    <div id="log">まだ操作はありません。</div>
  </section>

  <section class="panel">
    <h2>相手の手札</h2>
    <div id="opponentHand" class="cards"></div>
  </section>

  <div id="fields"></div>

  <section class="panel">
    <h2>あなたの手札</h2>
    <div id="hand" class="cards"></div>
  </section>
</main>

<div id="menuOverlay" class="overlay"
  role="dialog" aria-modal="true"
  aria-labelledby="menuTitle">

  <div class="dialog">
    <h2 id="menuTitle">∇ ナブラ演算子ゲーム（一人用）</h2>

    <p>
      関数や演算子のカードを使い、
      相手の場を先に０次元にするゲームです。
    </p>

    <div class="actions">
      <label for="menuDifficulty">相手の強さ</label>

      <select id="menuDifficulty">
        <option value="easy">初級</option>
        <option value="normal" selected>中級</option>
        <option value="hard">上級</option>
        <option value="expert">超級</option>
      </select>
    </div>

    <div class="actions">
      <button id="menuStart" class="control" disabled>
        ゲームを始める
      </button>

      <button id="menuRules" class="control">
        遊び方を見る
      </button>

      <button id="menuReturn" class="control" hidden>
        ゲームに戻る
      </button>
    </div>

    <p id="menuMessage" role="status">
      ゲームの準備をしています…
    </p>

    <div id="rulesBox" hidden>
      <h3>遊び方</h3>

      <ol>
        <li>
          自分と相手の場は、どちらも
          $1$、$x$、$x^2$ から始まります。
        </li>
        <li>
          手札を選び、操作したい場または基底を押します。
          先攻のあなたから交互に操作します。
        </li>
        <li>
          関数カードは場に基底を追加します。
          ∇は場全体を１回、△は場全体を２回微分します。
        </li>
        <li>
          $d/dx$・$\int$ は同じ基底に使う
          同種カードを追加選択できます。
          ×・÷は関数カードも選びます。
        </li>
        <li>
          演算で０になった基底、発散した基底、
          同じ場の別の基底の定数倍になった基底は消えます。
        </li>
        <li>
          相手の場を先に０次元にすると勝利です。
          山札が尽きた場合は場の次元を比較します。
        </li>
      </ol>

      <p class="hint">
        説明書を参考にした試作版です。
        計算結果を確認できない操作は、
        カードを消費せずに拒否します。
      </p>
    </div>
  </div>
</div>

<div id="resultOverlay" class="overlay" hidden
  role="dialog" aria-modal="true"
  aria-labelledby="resultTitle">

  <div class="dialog">
    <h2 id="resultTitle">結果</h2>
    <p id="resultText"></p>

    <div class="actions">
      <button id="resultReset" class="control">
        もう一度
      </button>

      <button id="resultClose" class="control">
        盤面を見る
      </button>

      <button id="resultMenu" class="control">
        メニューへ
      </button>
    </div>
  </div>
</div>

<script
  src="https://cdn.jsdelivr.net/pyodide/v0.27.4/full/pyodide.js">
</script>

<script id="pythonSource" type="text/plain">
import json
import random
import sympy as sp
from sympy.calculus.util import continuous_domain

x = sp.Symbol("x", real=True)

FUNCTIONS = {
    "cos": sp.cos(x),
    "sin": sp.sin(x),
    "exp": sp.exp(x),
    "x": x,
    "x2": x**2,
    "one": sp.Integer(1),
    "zero": sp.Integer(0)
}

META = {
    "cos": ("cos x", r"\cos x", "function"),
    "sin": ("sin x", r"\sin x", "function"),
    "exp": ("e^x", r"e^x", "function"),
    "x": ("x", r"x", "function"),
    "x2": ("x²", r"x^2", "function"),
    "one": ("1", r"1", "function"),
    "zero": ("0", r"0", "function"),

    "nabla": ("∇", r"\nabla", "global"),
    "triangle": ("△", r"\triangle", "global"),

    "derive": ("d/dx", r"\dfrac{d}{dx}", "repeat"),
    "integrate": ("∫", r"\int", "repeat"),

    "multiply": ("×", r"\times", "combine"),
    "divide": ("÷", r"\div", "combine"),

    "limPlus": (
        "lim x→+∞",
        r"\lim\limits_{\scriptstyle x\to+\infty}",
        "single"
    ),
    "limMinus": (
        "lim x→−∞",
        r"\lim\limits_{\scriptstyle x\to-\infty}",
        "single"
    ),
    "lim0": (
        "lim x→0",
        r"\lim\limits_{\scriptstyle x\to0}",
        "single"
    ),
    "limsup": (
        "limsup x→+∞",
        r"\limsup\limits_{\scriptstyle x\to+\infty}",
        "single"
    ),
    "liminf": (
        "liminf x→+∞",
        r"\liminf\limits_{\scriptstyle x\to+\infty}",
        "single"
    ),

    "sqrt": (
        "√",
        r"\sqrt{\phantom{x}}",
        "single"
    ),
    "log": ("log", r"\log", "single")
}

# 説明書にカード枚数の指定がないため、
# 以前の仮設定を調整した合計79枚。
# 逆関数を除き、その３枚を lim0・limsup・liminf に追加。
COPIES = {
    "cos": 3,
    "sin": 3,
    "exp": 3,
    "x": 3,
    "x2": 3,
    "one": 3,
    "zero": 1,

    "nabla": 2,
    "triangle": 2,
    "derive": 8,
    "integrate": 7,
    "multiply": 7,
    "divide": 7,

    "limPlus": 4,
    "limMinus": 4,
    "lim0": 5,
    "limsup": 4,
    "liminf": 4,

    "sqrt": 3,
    "log": 3
}

assert sum(COPIES.values()) == 79

NAMES = {
    "easy": "初級",
    "normal": "中級",
    "hard": "上級",
    "expert": "超級"
}

game = None


class InvalidMove(Exception):
    pass


def label(card_id):
    return META[card_id][0]


def kind(card_id):
    return META[card_id][2]


def expression_text(expr):
    return str(expr).replace("**", "^")


def field_text(expressions):
    if not expressions:
        return "基底なし（0次元）"

    return (
        "、".join(
            expression_text(value)
            for value in expressions
        )
        + "（"
        + str(len(expressions))
        + "次元）"
    )


def cleaned(expressions):
    """0と、定数倍と確認できる重複基底を除く。"""
    result = []

    for value in expressions:
        current = sp.simplify(value)

        if current == 0:
            continue

        duplicate = False

        for existing in result:
            try:
                ratio = sp.simplify(
                    current / existing
                )

                if (
                    not ratio.free_symbols
                    and ratio.is_zero is False
                    and ratio.is_finite is True
                    and ratio.is_real is True
                    and sp.simplify(
                        current - ratio * existing
                    ) == 0
                ):
                    duplicate = True
                    break
            except Exception:
                pass

        if not duplicate:
            result.append(current)

    return result


def check_real(value):
    """実数として確認できない操作を拒否する。"""
    value = sp.simplify(value)

    if value.has(sp.zoo, sp.nan):
        raise InvalidMove(
            "実数の式として扱えません。"
        )

    if value.is_real is False:
        raise InvalidMove(
            "実数の範囲で定義できません。"
        )

    if not value.free_symbols:
        if value.is_real is not True:
            raise InvalidMove(
                "実数の値と確認できません。"
            )

        return value

    try:
        domain = continuous_domain(
            value, x, sp.S.Reals
        )

        if domain is sp.S.EmptySet:
            raise InvalidMove(
                "実数の定義域がありません。"
            )

    except InvalidMove:
        raise

    except Exception:
        raise InvalidMove(
            "実数の定義域を確認できませんでした。"
        )

    return value


def finite_limit(value):
    if (
        isinstance(value, sp.Limit)
        or value.has(sp.Limit, sp.nan, sp.zoo)
    ):
        raise InvalidMove(
            "極限を確定できませんでした。"
        )

    if value in (sp.oo, -sp.oo):
        return None

    if isinstance(value, sp.AccumBounds):
        raise InvalidMove(
            "振動する通常の極限は取れません。"
        )

    if (
        value.is_real is not True
        or value.is_finite is not True
    ):
        raise InvalidMove(
            "実数の有限な極限と確認できません。"
        )

    return sp.simplify(value)


def take_limit(expr, card_id):
    try:
        if card_id == "lim0":
            domain = continuous_domain(
                expr, x, sp.S.Reals
            )

            if domain.contains(0) is not sp.S.true:
                raise InvalidMove(
                    "x=0が定義域に含まれません。"
                )

            left = sp.limit(
                expr, x, 0, dir="-"
            )
            right = sp.limit(
                expr, x, 0, dir="+"
            )

            if left != right:
                if sp.simplify(left - right) != 0:
                    raise InvalidMove(
                        "左右の極限が一致しません。"
                    )

            return finite_limit(right)

        if card_id == "limMinus":
            value = sp.limit(
                expr, x, -sp.oo
            )
        else:
            value = sp.limit(
                expr, x, sp.oo
            )

        if card_id in ("limsup", "liminf"):
            if isinstance(value, sp.AccumBounds):
                value = (
                    value.max
                    if card_id == "limsup"
                    else value.min
                )

        return finite_limit(value)

    except InvalidMove:
        raise

    except Exception:
        raise InvalidMove(
            "この極限の結果を確定できませんでした。"
        )


def transform(
    card_id,
    before,
    index,
    extra_ids=(),
    reverse_divide=False
):
    category = kind(card_id)

    if category in ("function", "global"):
        if index is not None:
            raise InvalidMove(
                "「この場に実行」を押してください。"
            )

        if extra_ids:
            raise InvalidMove(
                "追加カードは使えません。"
            )

        if category == "function":
            after = cleaned(
                before + [FUNCTIONS[card_id]]
            )

            return (
                after,
                label(card_id)
                + "を基底として追加"
            )

        order = (
            2 if card_id == "triangle" else 1
        )

        after = cleaned([
            sp.diff(value, x, order)
            for value in before
        ])

        return (
            after,
            "場の全基底を"
            + str(order)
            + "回微分"
        )

    if (
        not isinstance(index, int)
        or isinstance(index, bool)
        or index < 0
        or index >= len(before)
    ):
        raise InvalidMove(
            "対象の基底を押してください。"
        )

    original = before[index]

    if category == "repeat":
        if any(
            other != card_id
            for other in extra_ids
        ):
            raise InvalidMove(
                "同種の演算子だけ追加できます。"
            )

        changed = original

        for _ in range(1 + len(extra_ids)):
            if card_id == "derive":
                changed = sp.diff(
                    changed, x
                )
            else:
                changed = sp.integrate(
                    changed, x
                )

                if changed.has(sp.Integral):
                    raise InvalidMove(
                        "積分結果を確定できませんでした。"
                    )

    elif category == "combine":
        if not extra_ids:
            raise InvalidMove(
                "関数カードも選んでください。"
            )

        if any(
            kind(other) != "function"
            for other in extra_ids
        ):
            raise InvalidMove(
                "関数カードだけ組み合わせられます。"
            )

        product = sp.prod(
            FUNCTIONS[other]
            for other in extra_ids
        )

        if card_id == "multiply":
            changed = original * product

        else:
            if reverse_divide:
                numerator = product
                divisor = original
            else:
                numerator = original
                divisor = product

            if sp.simplify(divisor) == 0:
                raise InvalidMove(
                    "恒等的に0の式では割れません。"
                )

            # 個別の未定義点は、約分後の式で扱う
            # この試作版の仕様。
            changed = sp.cancel(
                numerator / divisor
            )

    else:
        if extra_ids:
            raise InvalidMove(
                "この演算子は１枚だけ使えます。"
            )

        if card_id.startswith("lim"):
            changed = take_limit(
                original, card_id
            )

            if changed is None:
                after = cleaned(
                    before[:index]
                    + before[index + 1:]
                )

                return (
                    after,
                    expression_text(original)
                    + "の極限が発散 → 基底を消去"
                )

        elif card_id == "sqrt":
            changed = sp.sqrt(original)

        elif card_id == "log":
            changed = sp.log(original)

        else:
            raise InvalidMove(
                "未対応のカードです。"
            )

    changed = check_real(changed)

    after = list(before)
    after[index] = changed
    after = cleaned(after)

    detail = (
        expression_text(original)
        + " → "
        + expression_text(changed)
    )

    if changed == 0:
        detail += "（0になったため消去）"
    elif len(after) < len(before):
        detail += "（重複する基底を消去）"

    return after, detail


def history_text(
    turn,
    actor,
    side,
    cards,
    before,
    index,
    after,
    detail,
    reverse=False
):
    owner = (
        "あなた"
        if side == "human"
        else "相手"
    )

    used = " ＋ ".join(
        "「" + label(card_id) + "」"
        for card_id in cards
    )

    target = (
        "場全体"
        if index is None
        else "基底「"
        + expression_text(before[index])
        + "」"
    )

    direction = (
        "（関数÷基底）"
        if reverse and cards[0] == "divide"
        else ""
    )

    return (
        str(turn)
        + "手目・"
        + actor
        + "\n使用カード："
        + used
        + direction
        + "\n対象："
        + owner
        + "の場の"
        + target
        + "\n操作結果："
        + detail
        + "\n操作後の"
        + owner
        + "の場："
        + field_text(after)
    )


def make_deck():
    deck = []

    for card_id, copies in COPIES.items():
        deck.extend(
            [card_id] * copies
        )

    random.shuffle(deck)
    return deck


def draw(player, amount):
    for _ in range(amount):
        if not game["deck"]:
            break

        game["hands"][player].append(
            game["deck"].pop()
        )


def spend(player, indices):
    unique = set(indices)

    for index in sorted(
        unique, reverse=True
    ):
        card_id = game["hands"][player].pop(
            index
        )

        game["discard"].append(
            card_id
        )

    draw(player, len(unique))


def ending():
    if not game["fields"]["ai"]:
        game["gameOver"] = True

        return (
            "勝利！ 相手の場が"
            "0次元になりました。"
        )

    if not game["fields"]["human"]:
        game["gameOver"] = True

        return (
            "敗北。あなたの場が"
            "0次元になりました。"
        )

    if not game["deck"]:
        game["gameOver"] = True

        own = len(game["fields"]["human"])
        opponent = len(game["fields"]["ai"])

        if own > opponent:
            outcome = "あなたの勝利"
        elif own < opponent:
            outcome = "相手の勝利"
        else:
            outcome = "引き分け（試作ルール）"

        return (
            "山札終了："
            + outcome
            + "。あなた"
            + str(own)
            + "次元、相手"
            + str(opponent)
            + "次元。"
        )

    return None


def state_for_browser():
    return {
        "deck": len(game["deck"]),
        "discard": len(game["discard"]),
        "aiHand": len(
            game["hands"]["ai"]
        ),
        "difficulty": game["difficulty"],

        "hand": [
            {
                "id": card_id,
                "kind": kind(card_id),
                "label": label(card_id),
                "tex": META[card_id][1]
            }
            for card_id in
            game["hands"]["human"]
        ],

        "fields": {
            side: [
                sp.latex(value)
                for value in
                game["fields"][side]
            ]
            for side in ("human", "ai")
        },

        "log": game["log"][:40],
        "gameOver": game["gameOver"]
    }


def reset_game(difficulty):
    global game

    if difficulty not in NAMES:
        raise InvalidMove(
            "難易度を選んでください。"
        )

    game = {
        "deck": make_deck(),
        "discard": [],

        "hands": {
            "human": [],
            "ai": []
        },

        "fields": {
            "human": [
                sp.Integer(1),
                x,
                x**2
            ],
            "ai": [
                sp.Integer(1),
                x,
                x**2
            ]
        },

        "difficulty": difficulty,
        "turn": 1,
        "phase": "human",
        "gameOver": False,
        "plan": None,
        "planned": False,
        "log": []
    }

    draw("human", 7)
    draw("ai", 7)


def validate_selection(
    hand,
    selected,
    extras
):
    if (
        not isinstance(selected, int)
        or isinstance(selected, bool)
        or selected < 0
        or selected >= len(hand)
    ):
        raise InvalidMove(
            "先にカードを選んでください。"
        )

    if not isinstance(extras, list):
        raise InvalidMove(
            "追加カードの指定が不正です。"
        )

    if (
        any(
            not isinstance(i, int)
            or isinstance(i, bool)
            or i < 0
            or i >= len(hand)
            for i in extras
        )
        or len(set(extras)) != len(extras)
        or selected in extras
    ):
        raise InvalidMove(
            "追加カードの指定が不正です。"
        )

    return (
        hand[selected],
        [hand[i] for i in extras]
    )


def human_play(request):
    if game is None or game["gameOver"]:
        raise InvalidMove(
            "ゲームは終了しています。"
        )

    if game["phase"] != "human":
        raise InvalidMove(
            "現在は相手のターンです。"
        )

    hand = game["hands"]["human"]
    selected = request.get("selected")
    extras = request.get("extras", [])
    side = request.get("side")
    index = request.get("index")

    if side not in ("human", "ai"):
        raise InvalidMove(
            "対象の場を選んでください。"
        )

    card_id, extra_ids = validate_selection(
        hand, selected, extras
    )

    before = list(
        game["fields"][side]
    )

    reverse = (
        request.get("reverseDivide")
        is True
    )

    after, detail = transform(
        card_id,
        before,
        index,
        extra_ids,
        reverse
    )

    entry = history_text(
        game["turn"],
        "あなた",
        side,
        [card_id] + extra_ids,
        before,
        index,
        after,
        detail,
        reverse
    )

    game["fields"][side] = after
    spend(
        "human",
        [selected] + extras
    )
    game["log"].insert(0, entry)

    message = ending()

    if message:
        game["phase"] = "finished"
        return message

    game["phase"] = "ai"
    game["plan"] = None
    game["planned"] = False

    return (
        "あなたの操作が確定しました。"
        "相手のターンです。"
    )


def ai_candidates():
    hand = game["hands"]["ai"]

    for card_index, card_id in enumerate(
        hand
    ):
        category = kind(card_id)

        for side in ("human", "ai"):
            field = game["fields"][side]

            indices = (
                [None]
                if category in
                ("function", "global")
                else range(len(field))
            )

            for index in indices:
                choices = [([], False)]

                if category == "combine":
                    factors = [
                        i
                        for i, other in
                        enumerate(hand)
                        if (
                            i != card_index
                            and kind(other)
                            == "function"
                        )
                    ]

                    choices = [
                        ([i], reverse)
                        for i in factors
                        for reverse in (
                            (False, True)
                            if card_id == "divide"
                            else (False,)
                        )
                    ]

                elif category == "repeat":
                    same = [
                        i
                        for i, other in
                        enumerate(hand)
                        if (
                            i != card_index
                            and other == card_id
                        )
                    ]

                    choices += [
                        ([i], False)
                        for i in same[:2]
                    ]

                for extras, reverse in choices:
                    yield {
                        "card_index": card_index,
                        "side": side,
                        "index": index,
                        "extras": extras,
                        "reverse": reverse
                    }


def base_score(candidate):
    side = candidate["side"]
    old = len(game["fields"][side])
    new = len(candidate["after"])

    if side == "human":
        if new == 0:
            return 100000

        return (old - new) * 100

    defense_weight = (
        8
        if game["difficulty"] == "normal"
        else 55
    )

    return (
        new - old
    ) * defense_weight


def imagined_reply(fields):
    """
    超級の限定的な先読み。
    プレイヤーの実際の手札は参照しない。
    """
    imagined = [
        "nabla", "triangle",
        "derive", "integrate",
        "limPlus", "limMinus",
        "lim0", "limsup", "liminf",
        "sqrt", "log",
        "zero", "one", "x", "x2",
        "sin", "cos", "exp"
    ]

    worst = 0
    trials = 0

    for side in ("ai", "human"):
        before = fields[side]

        for card_id in imagined:
            category = kind(card_id)

            indices = (
                [None]
                if category in
                ("function", "global")
                else range(len(before))
            )

            for index in indices:
                if (
                    card_id == "integrate"
                    and index is not None
                    and sp.count_ops(
                        before[index]
                    ) > 6
                ):
                    continue

                trials += 1

                if trials > 55:
                    return worst

                try:
                    after, _ = transform(
                        card_id,
                        before,
                        index
                    )
                except Exception:
                    continue

                if side == "ai":
                    risk = (
                        len(before)
                        - len(after)
                    ) * 100

                    if not after:
                        risk += 10000
                else:
                    risk = (
                        len(after)
                        - len(before)
                    ) * 55

                worst = max(
                    worst, risk
                )

    return worst


def choose_ai_plan():
    choices = list(
        ai_candidates()
    )

    random.shuffle(choices)
    evaluated = []

    for choice in choices:
        hand = game["hands"]["ai"]
        card_id = hand[
            choice["card_index"]
        ]

        extra_ids = [
            hand[i]
            for i in choice["extras"]
        ]

        try:
            after, detail = transform(
                card_id,
                game["fields"][
                    choice["side"]
                ],
                choice["index"],
                extra_ids,
                choice["reverse"]
            )
        except Exception:
            continue

        candidate = dict(choice)
        candidate["after"] = after
        candidate["detail"] = detail

        if game["difficulty"] == "easy":
            return candidate

        candidate["score"] = base_score(
            candidate
        )

        evaluated.append(
            candidate
        )

        if candidate["score"] == 100000:
            return candidate

    if not evaluated:
        return None

    evaluated.sort(
        key=lambda item: item["score"],
        reverse=True
    )

    if game["difficulty"] != "expert":
        return evaluated[0]

    best = None
    best_value = None

    for candidate in evaluated[:3]:
        hypothetical = {
            "human": (
                candidate["after"]
                if candidate["side"]
                == "human"
                else game["fields"]["human"]
            ),
            "ai": (
                candidate["after"]
                if candidate["side"]
                == "ai"
                else game["fields"]["ai"]
            )
        }

        risk = imagined_reply(
            hypothetical
        )

        value = (
            candidate["score"]
            - risk
        )

        if best is None or value > best_value:
            best = candidate
            best_value = value

    return best


def prepare_ai():
    if game is None or game["gameOver"]:
        raise InvalidMove(
            "ゲームは終了しています。"
        )

    if game["phase"] != "ai":
        raise InvalidMove(
            "現在は相手のターンではありません。"
        )

    if not game["planned"]:
        game["plan"] = choose_ai_plan()
        game["planned"] = True

    plan = game["plan"]

    if plan is None:
        return None

    hand = game["hands"]["ai"]

    indices = (
        [plan["card_index"]]
        + plan["extras"]
    )

    return {
        "side": plan["side"],
        "index": plan["index"],
        "cards": [
            {
                "index": i,
                "label": label(hand[i])
            }
            for i in indices
        ]
    }


def commit_ai():
    if game is None or game["gameOver"]:
        raise InvalidMove(
            "ゲームは終了しています。"
        )

    if game["phase"] != "ai":
        raise InvalidMove(
            "現在は相手のターンではありません。"
        )

    if not game["planned"]:
        raise InvalidMove(
            "相手の手がまだ準備されていません。"
        )

    plan = game["plan"]

    if plan is None:
        game["log"].insert(
            0,
            str(game["turn"])
            + "手目・相手\n"
            + "実行可能な操作なし"
        )

    else:
        side = plan["side"]
        before = list(
            game["fields"][side]
        )
        after = plan["after"]

        used = (
            [plan["card_index"]]
            + plan["extras"]
        )

        hand = game["hands"]["ai"]

        cards = [
            hand[i]
            for i in used
        ]

        entry = history_text(
            game["turn"],
            "相手",
            side,
            cards,
            before,
            plan["index"],
            after,
            plan["detail"],
            plan["reverse"]
        )

        game["fields"][side] = after
        spend("ai", used)
        game["log"].insert(
            0, entry
        )

    game["plan"] = None
    game["planned"] = False

    message = ending()

    if message:
        game["phase"] = "finished"
        return message

    game["turn"] += 1
    game["phase"] = "human"

    return (
        "あなたの番です。"
        "手札からカードを選んでください。"
    )


def handle(request_json):
    try:
        request = json.loads(
            request_json
        )

        action = request.get("action")

        if action == "reset":
            difficulty = request.get(
                "difficulty",
                "normal"
            )

            reset_game(difficulty)

            return json.dumps(
                {
                    "state": state_for_browser(),
                    "message": (
                        NAMES[difficulty]
                        + "で開始しました。"
                        + "あなたの番です。"
                    )
                },
                ensure_ascii=False
            )

        if action == "human_play":
            message = human_play(
                request
            )

            return json.dumps(
                {
                    "state": state_for_browser(),
                    "message": message
                },
                ensure_ascii=False
            )

        if action == "ai_plan":
            move = prepare_ai()

            return json.dumps(
                {"move": move},
                ensure_ascii=False
            )

        if action == "ai_commit":
            message = commit_ai()

            return json.dumps(
                {
                    "state": state_for_browser(),
                    "message": message
                },
                ensure_ascii=False
            )

        raise InvalidMove(
            "不明な操作です。"
        )

    except InvalidMove as error:
        return json.dumps(
            {"error": str(error)},
            ensure_ascii=False
        )

    except Exception as error:
        return json.dumps(
            {
                "error": (
                    "処理を完了できませんでした："
                    + str(error)[:150]
                )
            },
            ensure_ascii=False
        )
</script>

<script>
(() => {
  "use strict";

  const $ = id => document.getElementById(id);

  const sleep = ms =>
    new Promise(resolve => setTimeout(resolve, ms));

  const nextPaint = () =>
    new Promise(resolve => {
      requestAnimationFrame(() => {
        requestAnimationFrame(resolve);
      });
    });

  const names = {
    easy: "初級",
    normal: "中級",
    hard: "上級",
    expert: "超級"
  };

  let pyodide;
  let ready = false;
  let state = null;
  let primary = null;
  let extras = [];
  let busy = false;
  let mathQueue = Promise.resolve();

  function setStatus(message) {
    $("status").textContent = message;
    $("notice").textContent = message;
  }

  function element(
    tag,
    className = "",
    text = ""
  ) {
    const node = document.createElement(tag);
    node.className = className;
    node.textContent = text;
    return node;
  }

  function actionButton(
    className,
    callback
  ) {
    const node = element(
      "button",
      className
    );

    node.type = "button";

    node.disabled =
      busy ||
      !state ||
      state.gameOver ||
      !$("menuOverlay").hidden;

    node.addEventListener(
      "click",
      callback
    );

    return node;
  }

  function card(
    top,
    middle,
    bottom,
    className,
    callback
  ) {
    const node = actionButton(
      "playing-card " + className,
      callback
    );

    node.append(
      element(
        "span",
        "card-top",
        top
      ),
      element(
        "span",
        "card-symbol",
        "\$" + middle + "\$"
      ),
      element(
        "span",
        "card-bottom",
        bottom
      )
    );

    return node;
  }

  function count(title, value) {
    const node = element(
      "div",
      "counter"
    );

    node.append(
      element("span", "", title),
      element(
        "strong",
        "",
        String(value)
      )
    );

    return node;
  }

  function togglePanel(
    panelId,
    buttonId,
    title
  ) {
    const panel = $(panelId);
    const button = $(buttonId);

    panel.hidden = !panel.hidden;

    button.setAttribute(
      "aria-expanded",
      String(!panel.hidden)
    );

    button.textContent =
      title +
      (panel.hidden ? "を開く" : "を閉じる");
  }

  function typeset(nodes) {
    /*
     * 数式描画を処理待ちにします。
     * MathJaxが読み込めない場合も、
     * ゲーム開始自体は止めません。
     */
    mathQueue = mathQueue
      .catch(() => {})
      .then(async () => {
        if (
          typeof window.MathJax?.typesetPromise
          !== "function"
        ) {
          return;
        }

        await window.MathJax.typesetPromise(
          nodes
        );
      })
      .catch(error => {
        console.warn(
          "数式表示:",
          error
        );
      });
  }

  function render(changed = []) {
    if (!state) return;

    const fields = $("fields");
    const hand = $("hand");

    fields.replaceChildren();
    hand.replaceChildren();

    $("counts").replaceChildren(
      count("山札", state.deck + "枚"),
      count(
        "捨て札",
        state.discard + "枚"
      ),
      count(
        "相手の手札",
        state.aiHand + "枚"
      ),
      count(
        "あなたの手札",
        state.hand.length + "枚"
      ),
      count(
        "難易度",
        names[state.difficulty]
        || "不明"
      )
    );

    $("opponentHand").replaceChildren();

    for (
      let i = 0;
      i < state.aiHand;
      i++
    ) {
      $("opponentHand").appendChild(
        element(
          "div",
          "card-back",
          "∇"
        )
      );
    }

    for (const [side, title] of [
      ["ai", "相手の場"],
      ["human", "あなたの場"]
    ]) {
      const field = element(
        "section",
        "field " +
        (side === "ai"
          ? "enemy "
          : "") +
        (changed.includes(side)
          ? "field-flash"
          : "")
      );

      field.dataset.side = side;

      const head = element(
        "div",
        "field-head"
      );

      const heading = element(
        "h2",
        "",
        title
      );

      heading.appendChild(
        element(
          "span",
          "dimension",
          state.fields[side].length
          + "次元"
        )
      );

      const whole = actionButton(
        "control",
        () => play(side, null)
      );

      whole.textContent =
        "この場に実行";

      head.append(
        heading,
        whole
      );

      const cards = element(
        "div",
        "cards"
      );

      if (!state.fields[side].length) {
        cards.appendChild(
          element(
            "div",
            "empty",
            "基底なし"
          )
        );
      }

      state.fields[side].forEach(
        (expression, index) => {
          cards.appendChild(
            card(
              side === "ai"
                ? "相手の基底"
                : "自分の基底",
              expression,
              "基底 " + (index + 1),
              (side === "ai"
                ? "enemy-card"
                : "mine-card")
              + (changed.includes(side)
                ? " changed"
                : ""),
              () => play(
                side, index
              )
            )
          );
        }
      );

      field.append(
        head,
        cards
      );

      fields.appendChild(
        field
      );
    }
    state.hand.forEach((item, index) => {
      const className =
        "hand-card" +
        (item.id.startsWith("lim")
          ? " limit-card"
          : "") +
        (index === primary
          ? " selected"
          : "") +
        (extras.includes(index)
          ? " extra"
          : "");

      hand.appendChild(
        card(
          item.kind === "function"
            ? "関数"
            : "演算子",
          item.tex,
          "手札 " + (index + 1),
          className,
          () => choose(index)
        )
      );
    });

    const log = $("log");
    log.replaceChildren();

    if (!state.log.length) {
      log.textContent =
        "まだ操作はありません。";
    } else {
      for (const entry of state.log) {
        log.appendChild(
          element(
            "div",
            "log-entry",
            entry
          )
        );
      }
    }

    $("cancel").disabled =
      busy ||
      state.gameOver ||
      !$("menuOverlay").hidden;

    $("reset").disabled =
      busy || !ready;

    $("openMenu").disabled = busy;
    $("quickRules").disabled = busy;

    typeset([fields, hand]);
  }

  function choose(index) {
    if (
      busy ||
      !state ||
      state.gameOver
    ) {
      return;
    }

    const item = state.hand[index];

    if (primary === index) {
      primary = null;
      extras = [];

      setStatus(
        "選択を解除しました。"
      );

      render();
      return;
    }

    if (primary !== null) {
      const main =
        state.hand[primary];

      const factor =
        main.kind === "combine" &&
        item.kind === "function";

      const repeat =
        ["derive", "integrate"]
          .includes(main.id) &&
        item.id === main.id;

      if (factor || repeat) {
        extras = extras.includes(index)
          ? extras.filter(i => i !== index)
          : [...extras, index];

        setStatus(
          "対象の基底を押してください。"
        );

        render();
        return;
      }
    }

    primary = index;
    extras = [];

    setStatus(
      item.kind === "combine"
        ? "使う関数カードを選び、対象の基底を押してください。"
        : "対象の場または基底を押してください。"
    );

    render();
  }

  async function python(request) {
    pyodide.globals.set(
      "browser_request_json",
      JSON.stringify(request)
    );

    const result =
      await pyodide.runPythonAsync(
        "handle(browser_request_json)"
      );

    return JSON.parse(result);
  }

  function differences(before, after) {
    return ["human", "ai"].filter(
      side =>
        JSON.stringify(
          before.fields[side]
        ) !==
        JSON.stringify(
          after.fields[side]
        )
    );
  }

  async function fly(
    source,
    target,
    label,
    enemy = false
  ) {
    if (
      !source ||
      !target ||
      matchMedia(
        "(prefers-reduced-motion: reduce)"
      ).matches
    ) {
      await sleep(80);
      return;
    }

    const from =
      source.getBoundingClientRect();

    const to =
      target.getBoundingClientRect();

    const node = element(
      "div",
      "fly-card" +
      (enemy ? " enemy-fly" : ""),
      label
    );

    const startX =
      from.left + from.width / 2;

    const startY =
      from.top + from.height / 2;

    const endX =
      to.left + to.width / 2;

    const endY =
      to.top + to.height / 2;

    node.style.left =
      (startX - 48) + "px";

    node.style.top =
      (startY - 64) + "px";

    node.style.setProperty(
      "--dx",
      (endX - startX) + "px"
    );

    node.style.setProperty(
      "--dy",
      (endY - startY) + "px"
    );

    document.body.appendChild(node);

    requestAnimationFrame(() => {
      requestAnimationFrame(() => {
        node.classList.add("go");
      });
    });

    await sleep(650);
    node.remove();
  }

  function targetNode(side, index) {
    const field =
      document.querySelector(
        `.field[data-side="${side}"]`
      );

    if (index === null) {
      return field;
    }

    return (
      field?.querySelectorAll(
        ".playing-card"
      )[index] ||
      field
    );
  }

  async function animateHuman(
    side,
    index,
    used,
    oldHand
  ) {
    const target =
      targetNode(side, index);

    const cards =
      $("hand").querySelectorAll(
        ".playing-card"
      );

    await Promise.all(
      used.map(i =>
        fly(
          cards[i],
          target,
          oldHand[i]?.label || "カード"
        )
      )
    );
  }

  async function animateAI(move) {
    if (!move) {
      setStatus(
        "相手は実行可能な操作がありませんでした。"
      );

      await sleep(200);
      return;
    }

    const backs =
      $("opponentHand").querySelectorAll(
        ".card-back"
      );

    const target = targetNode(
      move.side,
      move.index
    );

    setStatus(
      "相手は「" +
      move.cards
        .map(card => card.label)
        .join("」「") +
      "」を使用！"
    );

    await Promise.all(
      move.cards.map(item =>
        fly(
          backs[item.index] || backs[0],
          target,
          item.label,
          true
        )
      )
    );
  }

  function showResult(message) {
    $("resultTitle").textContent =
      message.startsWith("勝利") ||
      message.includes("あなたの勝利")
        ? "勝利！"
        : message.startsWith("敗北") ||
          message.includes("相手の勝利")
          ? "敗北"
          : "ゲーム終了";

    $("resultText").textContent =
      message;

    $("resultOverlay").hidden =
      false;
  }

  async function play(side, index) {
    if (
      busy ||
      !state ||
      state.gameOver
    ) {
      return;
    }

    if (primary === null) {
      setStatus(
        "先に手札を選んでください。"
      );

      return;
    }

    const selected = primary;
    const additional = [...extras];

    busy = true;
    render();

    try {
      const answer = await python({
        action: "human_play",
        side,
        index,
        selected,
        extras: additional,
        reverseDivide:
          $("reverseDivide").checked
      });

      if (answer.error) {
        setStatus(
          "操作できません：" +
          answer.error +
          "\nカードは消費していません。"
        );

        return;
      }

      const before = state;

      await animateHuman(
        side,
        index,
        [selected, ...additional],
        before.hand
      );

      state = answer.state;
      primary = null;
      extras = [];

      render(
        differences(before, state)
      );

      setStatus(
        state.gameOver
          ? answer.message
          : "あなたの操作結果を表示しました。"
      );

      await nextPaint();

      if (state.gameOver) {
        showResult(answer.message);
        return;
      }

      await sleep(1000);

      setStatus(
        "相手が考えています…"
      );

      await nextPaint();

      const plan = await python({
        action: "ai_plan"
      });

      if (plan.error) {
        throw new Error(
          plan.error
        );
      }

      await animateAI(plan.move);

      const beforeAI = state;

      const committed =
        await python({
          action: "ai_commit"
        });

      if (committed.error) {
        throw new Error(
          committed.error
        );
      }

      state = committed.state;

      render(
        differences(beforeAI, state)
      );

      setStatus(
        committed.message
      );

      if (state.gameOver) {
        showResult(
          committed.message
        );
      }

    } catch (error) {
      setStatus(
        "処理を完了できませんでした：" +
        error.message +
        "\n必要に応じて最初からやり直してください。"
      );

    } finally {
      busy = false;
      render();
    }
  }

  async function startGame() {
    if (busy || !ready) {
      return;
    }

    busy = true;

    $("menuStart").disabled =
      true;

    $("resultOverlay").hidden =
      true;

    $("menuMessage").textContent =
      "新しいゲームを準備しています…";

    try {
      const answer = await python({
        action: "reset",
        difficulty:
          $("menuDifficulty").value
      });

      if (answer.error) {
        throw new Error(
          answer.error
        );
      }

      state = answer.state;
      primary = null;
      extras = [];

      $("difficulty").value =
        $("menuDifficulty").value;

      $("menuOverlay").hidden =
        true;

      render();
      await nextPaint();

      setStatus(
        answer.message
      );

    } catch (error) {
      $("menuMessage").textContent =
        "開始できませんでした：" +
        error.message;

      setStatus(
        "開始できませんでした：" +
        error.message
      );

    } finally {
      busy = false;

      $("menuStart").disabled =
        !ready;

      render();
    }
  }

  function openMenu() {
    if (busy) {
      return;
    }

    $("resultOverlay").hidden =
      true;

    $("menuOverlay").hidden =
      false;

    $("menuReturn").hidden =
      !state;

    $("menuDifficulty").value =
      $("difficulty").value;

    $("menuMessage").textContent =
      ready
        ? "難易度を選び、ゲームを始めてください。"
        : "ゲームの準備をしています…";

    $("menuStart").disabled =
      !ready;

    render();
  }

  function showRules() {
    const box = $("rulesBox");

    box.hidden = false;

    $("menuRules").textContent =
      "遊び方を閉じる";

    typeset([box]);
  }

  $("openSettings").addEventListener(
    "click",
    () => {
      togglePanel(
        "settingsPanel",
        "openSettings",
        "状況・設定"
      );
    }
  );

  $("openHistory").addEventListener(
    "click",
    () => {
      togglePanel(
        "historyPanel",
        "openHistory",
        "操作履歴"
      );
    }
  );

  $("menuDifficulty").addEventListener(
    "change",
    () => {
      $("difficulty").value =
        $("menuDifficulty").value;
    }
  );

  $("difficulty").addEventListener(
    "change",
    () => {
      $("menuDifficulty").value =
        $("difficulty").value;

      if (!busy && state) {
        setStatus(
          "難易度の変更は、新しいゲームから反映されます。"
        );
      }
    }
  );

  $("menuStart").addEventListener(
    "click",
    startGame
  );

  $("menuRules").addEventListener(
    "click",
    () => {
      const box = $("rulesBox");

      if (box.hidden) {
        showRules();
      } else {
        box.hidden = true;

        $("menuRules").textContent =
          "遊び方を見る";
      }
    }
  );

  $("menuReturn").addEventListener(
    "click",
    () => {
      if (!state || busy) {
        return;
      }

      $("menuOverlay").hidden =
        true;

      render();
    }
  );

  $("openMenu").addEventListener(
    "click",
    openMenu
  );

  $("quickRules").addEventListener(
    "click",
    () => {
      if (busy) {
        return;
      }

      openMenu();
      showRules();
    }
  );

  $("cancel").addEventListener(
    "click",
    () => {
      primary = null;
      extras = [];

      setStatus(
        "選択を解除しました。"
      );

      render();
    }
  );

  $("reset").addEventListener(
    "click",
    startGame
  );

  $("resultReset").addEventListener(
    "click",
    startGame
  );

  $("resultClose").addEventListener(
    "click",
    () => {
      $("resultOverlay").hidden =
        true;
    }
  );

  $("resultMenu").addEventListener(
    "click",
    openMenu
  );

  async function initialize() {
    try {
      $("menuMessage").textContent =
        "Pyodideを読み込んでいます…";

      setStatus(
        "計算環境を読み込んでいます…"
      );

      if (
        typeof loadPyodide ===
        "undefined"
      ) {
        throw new Error(
          "Pyodideを読み込めませんでした。"
        );
      }

      pyodide = await loadPyodide({
        indexURL:
          "https://cdn.jsdelivr.net/pyodide/v0.27.4/full/"
      });

      $("menuMessage").textContent =
        "計算ライブラリを読み込んでいます…";

      setStatus(
        "数学ライブラリを読み込んでいます…"
      );

      try {
        await pyodide.loadPackage(
          "sympy"
        );

      } catch (_) {
        await pyodide.loadPackage(
          "micropip"
        );

        await pyodide.runPythonAsync(
          "import micropip; " +
          "await micropip.install('sympy')"
        );
      }

      $("menuMessage").textContent =
        "ゲームのルールを準備しています…";

      await pyodide.runPythonAsync(
        $("pythonSource").textContent
      );

      ready = true;

      $("menuStart").disabled =
        false;

      $("menuMessage").textContent =
        "準備ができました。ゲームを始めてください。";

      setStatus(
        "メニューからゲームを始めてください。"
      );

    } catch (error) {
      ready = false;

      $("menuStart").disabled =
        true;

      $("menuMessage").textContent =
        "起動できませんでした：" +
        error.message +
        "\n通信環境を確認して再読み込みしてください。";

      setStatus(
        $("menuMessage").textContent
      );
    }
  }

  initialize();
})();
</script>
</body>
</html>
