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
  margin: 8px 0 14px;
  color: var(--gold);
  text-align: center;
  font-size: clamp(23px, 4vw, 34px);
}

h2 {
  margin: 0 0 10px;
  color: var(--gold);
  font-size: 18px;
}

button, select { font: inherit; }
button { cursor: pointer; }
button:disabled { cursor: default; opacity: .55; }

button:focus-visible,
select:focus-visible,
input:focus-visible {
  outline: 3px solid white;
  outline-offset: 3px;
}

[hidden] { display: none !important; }

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
  min-height: 205px;
  margin: 11px 0;
  padding: 14px;
  border: 2px solid #a6c8ac;
  border-radius: 16px;
  background: #244b3d;
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
  min-height: 160px;
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
  display: grid;
  place-items: center;
  width: 100%;
  max-height: 112px;
  overflow: auto;
  font-size: 21px;
  overflow-wrap: anywhere;
}

.field .card-symbol { font-size: 23px; }

.limit-card .card-symbol {
  font-size: 14px;
}

#hand {
  flex-wrap: nowrap;
  justify-content: flex-start;
  overflow-x: auto;
  padding: 7px 4px 12px;
}

#opponentHand {
  min-height: 75px;
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

#notice, #status {
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

.overlay {
  position: fixed;
  inset: 0;
  z-index: 100;
  display: grid;
  place-items: center;
  padding: 15px;
  background: #07150de8;
}

.dialog {
  width: min(100%, 560px);
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
  .field { padding: 10px; }
  .field .playing-card { width: 105px; min-height: 140px; }
  .field .card-symbol { font-size: 19px; }
  .playing-card { width: 89px; min-height: 125px; }
  .card-symbol { font-size: 17px; }
  .limit-card .card-symbol { font-size: 12px; }
  .card-back { width: 44px; height: 65px; }
}
</style>
</head>

<body>
<main>
  <h1>∇ ナブラ演算子ゲーム（一人用）</h1>

  <div id="counts" class="topbar">
    ゲームの準備をしています…
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
          両者の場は「1」「x」「x²」から始まります。
          あなたが先攻です。
        </li>
        <li>
          使う手札を選び、操作する場または基底を押します。
        </li>
        <li>
          関数カードは「この場に実行」で追加します。
          ∇・△も「この場に実行」で場全体を微分します。
        </li>
        <li>
          d/dx・∫は同種カードを追加選択できます。
          ×・÷は使う関数カードも選択してください。
        </li>
        <li>
          「関数÷基底」を使う場合は、
          状況・設定でチェックを入れてください。
        </li>
        <li>
          ０になった基底、発散した基底、同じ場の
          別の基底の定数倍になった基底は消えます。
        </li>
        <li>
          先に相手を０次元にすると勝利です。
          山札がなくなった場合は次元を比較します。
        </li>
      </ol>

      <p>
        この一人用版は試作です。カード枚数は
        説明書で指定されていないため仮設定です。
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
    "cos": ("cos x", "function"),
    "sin": ("sin x", "function"),
    "exp": ("e^x", "function"),
    "x": ("x", "function"),
    "x2": ("x²", "function"),
    "one": ("1", "function"),
    "zero": ("0", "function"),
    "nabla": ("∇", "global"),
    "triangle": ("△", "global"),
    "derive": ("d/dx", "repeat"),
    "integrate": ("∫", "repeat"),
    "multiply": ("×", "combine"),
    "divide": ("÷", "combine"),
    "limPlus": ("lim x→+∞", "single"),
    "limMinus": ("lim x→−∞", "single"),
    "lim0": ("lim x→0", "single"),
    "limsup": ("limsup x→+∞", "single"),
    "liminf": ("liminf x→+∞", "single"),
    "sqrt": ("√", "single"),
    "log": ("log", "single")
}

# 逆関数は含めず、合計79枚の仮設定。
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
    return META[card_id][1]


def text(value):
    return str(value).replace("**", "^")


def field_text(values):
    if not values:
        return "基底なし（0次元）"

    return "、".join(map(text, values)) + (
        "（" + str(len(values)) + "次元）"
    )


def cleaned(values):
    result = []

    for value in values:
        current = sp.simplify(value)

        if current == 0:
            continue

        duplicate = False

        for previous in result:
            try:
                ratio = sp.simplify(current / previous)

                if (
                    not ratio.free_symbols
                    and ratio.is_real is True
                    and ratio.is_finite is True
                    and ratio.is_zero is False
                    and sp.simplify(
                        current - ratio * previous
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
    value = sp.simplify(value)

    if value.has(sp.nan, sp.zoo):
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
            "有限の実数値と確認できません。"
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

            left = sp.limit(expr, x, 0, dir="-")
            right = sp.limit(expr, x, 0, dir="+")

            if left != right:
                if sp.simplify(left - right) != 0:
                    raise InvalidMove(
                        "左右の極限が一致しません。"
                    )

            return finite_limit(right)

        point = (
            -sp.oo
            if card_id == "limMinus"
            else sp.oo
        )

        value = sp.limit(expr, x, point)

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
    extras=(),
    reverse=False
):
    category = kind(card_id)

    if category in ("function", "global"):
        if index is not None:
            raise InvalidMove(
                "「この場に実行」を押してください。"
            )

        if extras:
            raise InvalidMove(
                "追加カードは使えません。"
            )

        if category == "function":
            return (
                cleaned(
                    before + [FUNCTIONS[card_id]]
                ),
                label(card_id) + "を基底として追加"
            )

        order = (
            2 if card_id == "triangle" else 1
        )

        return (
            cleaned([
                sp.diff(value, x, order)
                for value in before
            ]),
            "場の全基底を" +
            str(order) +
            "回微分"
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
            extra != card_id
            for extra in extras
        ):
            raise InvalidMove(
                "同種の演算子だけ追加できます。"
            )

        changed = original

        for _ in range(1 + len(extras)):
            if card_id == "derive":
                changed = sp.diff(changed, x)
            else:
                changed = sp.integrate(changed, x)

                if changed.has(sp.Integral):
                    raise InvalidMove(
                        "積分結果を確定できませんでした。"
                    )

    elif category == "combine":
        if not extras:
            raise InvalidMove(
                "関数カードも選んでください。"
            )

        if any(
            kind(extra) != "function"
            for extra in extras
        ):
            raise InvalidMove(
                "関数カードだけ組み合わせられます。"
            )

        product = sp.prod(
            FUNCTIONS[extra]
            for extra in extras
        )

        if card_id == "multiply":
            changed = original * product
        else:
            numerator, divisor = (
                (product, original)
                if reverse
                else (original, product)
            )

            if sp.simplify(divisor) == 0:
                raise InvalidMove(
                    "恒等的に0の式では割れません。"
                )

            # この試作版では、約分後の式で判定する。
            changed = sp.cancel(
                numerator / divisor
            )

    else:
        if extras:
            raise InvalidMove(
                "この演算子は１枚だけ使えます。"
            )

        if card_id.startswith("lim"):
            changed = take_limit(
                original, card_id
            )

            if changed is None:
                return (
                    cleaned(
                        before[:index] +
                        before[index + 1:]
                    ),
                    text(original) +
                    "の極限が発散 → 基底を消去"
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
        text(original) +
        " → " +
        text(changed)
    )

    if changed == 0:
        detail += "（0になったため消去）"
    elif len(after) < len(before):
        detail += "（重複する基底を消去）"

    return after, detail


def history(
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
        else "基底「" +
        text(before[index]) +
        "」"
    )

    direction = (
        "（関数÷基底）"
        if reverse and cards[0] == "divide"
        else ""
    )

    return (
        str(game["turn"]) +
        "手目・" +
        actor +
        "\n使用カード：" +
        used +
        direction +
        "\n対象：" +
        owner +
        "の場の" +
        target +
        "\n操作結果：" +
        detail +
        "\n操作後の場：" +
        field_text(after)
    )


def draw(player, amount):
    for _ in range(amount):
        if not game["deck"]:
            break

        game["hands"][player].append(
            game["deck"].pop()
        )


def spend(player, indices):
    for index in sorted(
        set(indices),
        reverse=True
    ):
        game["discard"].append(
            game["hands"][player].pop(index)
        )

    draw(player, len(set(indices)))


def ending():
    if not game["fields"]["ai"]:
        game["over"] = True
        return "勝利！ 相手の場が0次元になりました。"

    if not game["fields"]["human"]:
        game["over"] = True
        return "敗北。あなたの場が0次元になりました。"

    if not game["deck"]:
        game["over"] = True

        own = len(game["fields"]["human"])
        enemy = len(game["fields"]["ai"])

        if own > enemy:
            outcome = "あなたの勝利"
        elif own < enemy:
            outcome = "相手の勝利"
        else:
            outcome = "引き分け（試作ルール）"

        return (
            "山札終了：" +
            outcome +
            "。あなた" +
            str(own) +
            "次元、相手" +
            str(enemy) +
            "次元。"
        )

    return None


def browser_state():
    return {
        "deck": len(game["deck"]),
        "discard": len(game["discard"]),
        "aiHand": len(game["hands"]["ai"]),
        "difficulty": game["difficulty"],
        "hand": [
            {
                "id": card_id,
                "kind": kind(card_id),
                "label": label(card_id)
            }
            for card_id in game["hands"]["human"]
        ],
        "fields": {
            side: [
                text(value)
                for value in game["fields"][side]
            ]
            for side in ("human", "ai")
        },
        "log": game["log"][:40],
        "gameOver": game["over"]
    }


def reset(difficulty):
    global game

    if difficulty not in NAMES:
        raise InvalidMove(
            "難易度を選んでください。"
        )

    deck = [
        card_id
        for card_id, amount in COPIES.items()
        for _ in range(amount)
    ]

    random.shuffle(deck)

    game = {
        "deck": deck,
        "discard": [],
        "hands": {
            "human": [],
            "ai": []
        },
        "fields": {
            "human": [sp.Integer(1), x, x**2],
            "ai": [sp.Integer(1), x, x**2]
        },
        "difficulty": difficulty,
        "turn": 1,
        "phase": "human",
        "over": False,
        "plan": None,
        "planned": False,
        "log": []
    }

    draw("human", 7)
    draw("ai", 7)


def validate_indices(hand, selected, extras):
    if (
        not isinstance(selected, int)
        or isinstance(selected, bool)
        or selected < 0
        or selected >= len(hand)
    ):
        raise InvalidMove(
            "先に手札を選んでください。"
        )

    if (
        not isinstance(extras, list)
        or any(
            not isinstance(i, int)
            or isinstance(i, bool)
            or i < 0
            or i >= len(hand)
            for i in extras
        )
        or len(extras) != len(set(extras))
        or selected in extras
    ):
        raise InvalidMove(
            "追加カードの指定が不正です。"
        )


def human_play(request):
    if game is None or game["over"]:
        raise InvalidMove(
            "ゲームは終了しています。"
        )

    if game["phase"] != "human":
        raise InvalidMove(
            "現在は相手のターンです。"
        )

    side = request.get("side")
    index = request.get("index")
    selected = request.get("selected")
    extras = request.get("extras", [])
    reverse = request.get("reverseDivide") is True

    if side not in ("human", "ai"):
        raise InvalidMove(
            "対象の場を選んでください。"
        )

    hand = game["hands"]["human"]

    validate_indices(
        hand, selected, extras
    )

    cards = [
        hand[i]
        for i in [selected] + extras
    ]

    before = list(game["fields"][side])

    after, detail = transform(
        cards[0],
        before,
        index,
        cards[1:],
        reverse
    )

    entry = history(
        "あなた",
        side,
        cards,
        before,
        index,
        after,
        detail,
        reverse
    )

    game["fields"][side] = after
    spend("human", [selected] + extras)
    game["log"].insert(0, entry)

    message = ending()

    if message:
        game["phase"] = "finished"
        return message

    game["phase"] = "ai"
    game["planned"] = False

    return "あなたの操作を表示しました。"


def ai_candidates():
    hand = game["hands"]["ai"]

    for chosen, card_id in enumerate(hand):
        category = kind(card_id)

        for side in ("human", "ai"):
            before = game["fields"][side]

            targets = (
                [None]
                if category in ("function", "global")
                else range(len(before))
            )

            for index in targets:
                options = [([], False)]

                if category == "combine":
                    factors = [
                        i
                        for i, other in enumerate(hand)
                        if (
                            i != chosen
                            and kind(other) == "function"
                        )
                    ]

                    options = [
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
                        for i, other in enumerate(hand)
                        if (
                            i != chosen
                            and other == card_id
                        )
                    ]

                    options += [
                        ([i], False)
                        for i in same[:2]
                    ]

                for extras, reverse in options:
                    try:
                        after, detail = transform(
                            card_id,
                            before,
                            index,
                            [hand[i] for i in extras],
                            reverse
                        )
                    except Exception:
                        continue

                    yield {
                        "chosen": chosen,
                        "extras": extras,
                        "reverse": reverse,
                        "side": side,
                        "index": index,
                        "after": after,
                        "detail": detail
                    }


def choose_ai():
    options = list(ai_candidates())

    if not options:
        return None

    difficulty = game["difficulty"]

    if difficulty == "easy":
        return random.choice(options)

    def score(move):
        side = move["side"]
        old = len(game["fields"][side])
        new = len(move["after"])

        if side == "human":
            if new == 0:
                return 100000

            return (old - new) * 100

        defense = (
            15
            if difficulty == "normal"
            else 60
        )

        return (new - old) * defense

    if difficulty == "expert":
        def expert_score(move):
            value = score(move)

            # 限定的な防御評価。
            if move["side"] == "ai":
                if len(move["after"]) > len(
                    game["fields"]["ai"]
                ):
                    value += 30

            return value

        return max(options, key=expert_score)

    return max(options, key=score)


def ai_plan():
    if game is None or game["phase"] != "ai":
        raise InvalidMove(
            "現在は相手のターンではありません。"
        )

    if not game["planned"]:
        game["plan"] = choose_ai()
        game["planned"] = True

    move = game["plan"]

    if move is None:
        return None

    hand = game["hands"]["ai"]

    return {
        "side": move["side"],
        "index": move["index"],
        "cards": [
            label(hand[i])
            for i in (
                [move["chosen"]] +
                move["extras"]
            )
        ]
    }


def ai_commit():
    if (
        game is None
        or game["phase"] != "ai"
        or not game["planned"]
    ):
        raise InvalidMove(
            "相手の操作はまだ準備できていません。"
        )

    move = game["plan"]

    if move is None:
        game["log"].insert(
            0,
            str(game["turn"]) +
            "手目・相手\n実行可能な操作なし"
        )

    else:
        side = move["side"]
        before = list(game["fields"][side])
        after = move["after"]

        used = [
            move["chosen"]
        ] + move["extras"]

        cards = [
            game["hands"]["ai"][i]
            for i in used
        ]

        entry = history(
            "相手",
            side,
            cards,
            before,
            move["index"],
            after,
            move["detail"],
            move["reverse"]
        )

        game["fields"][side] = after
        spend("ai", used)
        game["log"].insert(0, entry)

    game["plan"] = None
    game["planned"] = False

    message = ending()

    if message:
        game["phase"] = "finished"
        return message

    game["turn"] += 1
    game["phase"] = "human"

    return "あなたの番です。"


def handle(raw):
    try:
        request = json.loads(raw)
        action = request.get("action")

        if action == "reset":
            difficulty = request.get(
                "difficulty", "normal"
            )

            reset(difficulty)

            return json.dumps({
                "state": browser_state(),
                "message": (
                    NAMES[difficulty] +
                    "で開始しました。あなたの番です。"
                )
            }, ensure_ascii=False)

        if action == "human_play":
            message = human_play(request)

            return json.dumps({
                "state": browser_state(),
                "message": message
            }, ensure_ascii=False)

        if action == "ai_plan":
            return json.dumps({
                "move": ai_plan()
            }, ensure_ascii=False)

        if action == "ai_commit":
            message = ai_commit()

            return json.dumps({
                "state": browser_state(),
                "message": message
            }, ensure_ascii=False)

        raise InvalidMove("不明な操作です。")

    except InvalidMove as error:
        return json.dumps({
            "error": str(error)
        }, ensure_ascii=False)

    except Exception as error:
        return json.dumps({
            "error": (
                "処理を完了できませんでした：" +
                str(error)[:150]
            )
        }, ensure_ascii=False)
</script>

<script>
(() => {
  "use strict";

  const $ = id => document.getElementById(id);
  const sleep = ms =>
    new Promise(resolve => setTimeout(resolve, ms));

  const names = {
    easy: "初級",
    normal: "中級",
    hard: "上級",
    expert: "超級"
  };

  let pyodide = null;
  let ready = false;
  let state = null;
  let selected = null;
  let extras = [];
  let busy = false;

  function status(message) {
    $("notice").textContent = message;
    $("status").textContent = message;
  }

  function node(tag, className = "", text = "") {
    const item = document.createElement(tag);
    item.className = className;
    item.textContent = text;
    return item;
  }

  function button(className, callback) {
    const item = node("button", className);
    item.type = "button";
    item.disabled =
      busy ||
      !state ||
      state.gameOver ||
      !$("menuOverlay").hidden;

    item.addEventListener("click", callback);
    return item;
  }

  function card(
    top,
    middle,
    bottom,
    className,
    callback
  ) {
    const item = button(
      "playing-card " + className,
      callback
    );

    item.append(
      node("span", "card-top", top),
      node("span", "card-symbol", middle),
      node("span", "card-bottom", bottom)
    );

    return item;
  }

  function counter(title, value) {
    const item = node("div", "counter");

    item.append(
      node("span", "", title),
      node("strong", "", String(value))
    );

    return item;
  }

  function toggle(panelId, buttonId, name) {
    const panel = $(panelId);
    const control = $(buttonId);

    panel.hidden = !panel.hidden;

    control.setAttribute(
      "aria-expanded",
      String(!panel.hidden)
    );

    control.textContent =
      name +
      (panel.hidden ? "を開く" : "を閉じる");
  }

  function render() {
    if (!state) return;

    $("counts").replaceChildren(
      counter("山札", state.deck + "枚"),
      counter("捨て札", state.discard + "枚"),
      counter("相手の手札", state.aiHand + "枚"),
      counter(
        "あなたの手札",
        state.hand.length + "枚"
      ),
      counter(
        "難易度",
        names[state.difficulty] || "不明"
      )
    );

    const backs = $("opponentHand");
    backs.replaceChildren();

    for (let i = 0; i < state.aiHand; i++) {
      backs.appendChild(
        node("div", "card-back", "∇")
      );
    }

    const fields = $("fields");
    fields.replaceChildren();

    for (const [side, title] of [
      ["ai", "相手の場"],
      ["human", "あなたの場"]
    ]) {
      const field = node(
        "section",
        "field" + (side === "ai" ? " enemy" : "")
      );

      const head = node("div", "field-head");
      const heading = node("h2", "", title);

      heading.appendChild(
        node(
          "span",
          "dimension",
          state.fields[side].length + "次元"
        )
      );

      const whole = button(
        "control",
        () => play(side, null)
      );

      whole.textContent = "この場に実行";
      head.append(heading, whole);

      const cards = node("div", "cards");

      if (!state.fields[side].length) {
        cards.appendChild(
          node("div", "empty", "基底なし")
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
              side === "ai"
                ? "enemy-card"
                : "mine-card",
              () => play(side, index)
            )
          );
        }
      );

      field.append(head, cards);
      fields.appendChild(field);
    }

    const hand = $("hand");
    hand.replaceChildren();

    state.hand.forEach((item, index) => {
      const styles =
        "hand-card" +
        (item.id.startsWith("lim")
          ? " limit-card"
          : "") +
        (selected === index
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
          item.label,
          "手札 " + (index + 1),
          styles,
          () => choose(index)
        )
      );
    });

    const log = $("log");
    log.replaceChildren();

    if (!state.log.length) {
      log.textContent = "まだ操作はありません。";
    } else {
      for (const entry of state.log) {
        log.appendChild(
          node("div", "log-entry", entry)
        );
      }
    }

    $("cancel").disabled =
      busy ||
      state.gameOver ||
      !$("menuOverlay").hidden;

    $("reset").disabled = busy || !ready;
    $("openMenu").disabled = busy;
    $("quickRules").disabled = busy;
  }

  function choose(index) {
    if (busy || !state || state.gameOver) return;

    const item = state.hand[index];

    if (selected === index) {
      selected = null;
      extras = [];
      status("選択を解除しました。");
      render();
      return;
    }

    if (selected !== null) {
      const main = state.hand[selected];

      const isFactor =
        main.kind === "combine" &&
        item.kind === "function";

      const isRepeat =
        ["derive", "integrate"].includes(main.id) &&
        item.id === main.id;

      if (isFactor || isRepeat) {
        extras = extras.includes(index)
          ? extras.filter(i => i !== index)
          : [...extras, index];

        status("対象の基底を押してください。");
        render();
        return;
      }
    }

    selected = index;
    extras = [];

    status(
      item.kind === "combine"
        ? "使う関数カードを追加選択し、対象の基底を押してください。"
        : "対象の場または基底を押してください。"
    );

    render();
  }

  async function python(request) {
    pyodide.globals.set(
      "browser_request_json",
      JSON.stringify(request)
    );

    const raw = await pyodide.runPythonAsync(
      "handle(browser_request_json)"
    );

    return JSON.parse(raw);
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

    $("resultText").textContent = message;
    $("resultOverlay").hidden = false;
  }

  async function play(side, index) {
    if (busy || !state || state.gameOver) return;

    if (selected === null) {
      status("先に手札を選んでください。");
      return;
    }

    busy = true;
    render();

    try {
      const result = await python({
        action: "human_play",
        side,
        index,
        selected,
        extras,
        reverseDivide: $("reverseDivide").checked
      });

      if (result.error) {
        status(
          "操作できません：" +
          result.error +
          "\nカードは消費していません。"
        );
        return;
      }

      state = result.state;
      selected = null;
      extras = [];

      render();
      status(result.message);

      if (state.gameOver) {
        showResult(result.message);
        return;
      }

      // あなたの操作結果を画面に描画してからAIを動かす。
      await new Promise(resolve =>
        requestAnimationFrame(() =>
          requestAnimationFrame(resolve)
        )
      );

      await sleep(1000);
      status("相手が考えています…");

      const planned = await python({
        action: "ai_plan"
      });

      if (planned.error) {
        throw new Error(planned.error);
      }

      if (planned.move) {
        status(
          "相手は" +
          planned.move.cards
            .map(name => "「" + name + "」")
            .join("＋") +
          "を使用しています…"
        );

        await sleep(400);
      }

      const committed = await python({
        action: "ai_commit"
      });

      if (committed.error) {
        throw new Error(committed.error);
      }

      state = committed.state;
      render();
      status(committed.message);

      if (state.gameOver) {
        showResult(committed.message);
      }

    } catch (error) {
      status(
        "処理を完了できませんでした：" +
        error.message
      );
    } finally {
      busy = false;
      render();
    }
  }

  async function startGame() {
    if (!ready || busy) return;

    busy = true;
    $("menuStart").disabled = true;
    $("resultOverlay").hidden = true;

    $("menuMessage").textContent =
      "新しいゲームを準備しています…";

    try {
      const result = await python({
        action: "reset",
        difficulty: $("menuDifficulty").value
      });

      if (result.error) {
        throw new Error(result.error);
      }

      state = result.state;
      selected = null;
      extras = [];

      $("difficulty").value =
        $("menuDifficulty").value;

      $("menuOverlay").hidden = true;

      status(result.message);

    } catch (error) {
      const message =
        "開始できませんでした：" +
        error.message;

      $("menuMessage").textContent = message;
      status(message);

    } finally {
      busy = false;
      $("menuStart").disabled = !ready;
      render();
    }
  }

  function openMenu(showRules = false) {
    if (busy) return;

    $("resultOverlay").hidden = true;
    $("menuOverlay").hidden = false;
    $("menuReturn").hidden = !state;

    $("menuDifficulty").value =
      $("difficulty").value;

    $("rulesBox").hidden = !showRules;

    $("menuRules").textContent = showRules
      ? "遊び方を閉じる"
      : "遊び方を見る";

    $("menuMessage").textContent = ready
      ? "難易度を選んでゲームを始めてください。"
      : "ゲームの準備をしています…";

    $("menuStart").disabled = !ready;
    render();
  }

  $("openSettings").addEventListener(
    "click",
    () => toggle(
      "settingsPanel",
      "openSettings",
      "状況・設定"
    )
  );

  $("openHistory").addEventListener(
    "click",
    () => toggle(
      "historyPanel",
      "openHistory",
      "操作履歴"
    )
  );

  $("openMenu").addEventListener(
    "click",
    () => openMenu(false)
  );

  $("quickRules").addEventListener(
    "click",
    () => openMenu(true)
  );

  $("menuRules").addEventListener(
    "click",
    () => {
      $("rulesBox").hidden =
        !$("rulesBox").hidden;

      $("menuRules").textContent =
        $("rulesBox").hidden
          ? "遊び方を見る"
          : "遊び方を閉じる";
    }
  );

  $("menuReturn").addEventListener(
    "click",
    () => {
      if (!state || busy) return;
      $("menuOverlay").hidden = true;
      render();
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

      status(
        "難易度の変更は次のゲームから反映されます。"
      );
    }
  );

  $("cancel").addEventListener(
    "click",
    () => {
      selected = null;
      extras = [];
      status("選択を解除しました。");
      render();
    }
  );

  $("menuStart").addEventListener(
    "click",
    startGame
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
      $("resultOverlay").hidden = true;
    }
  );

  $("resultMenu").addEventListener(
    "click",
    () => openMenu(false)
  );

  async function initialize() {
    try {
      $("menuMessage").textContent =
        "Pyodideを読み込んでいます…";

      status(
        "計算環境を読み込んでいます…"
      );

      if (typeof loadPyodide === "undefined") {
        throw new Error(
          "Pyodideの読み込みに失敗しました。"
        );
      }

      pyodide = await loadPyodide({
        indexURL:
          "https://cdn.jsdelivr.net/pyodide/v0.27.4/full/"
      });

      $("menuMessage").textContent =
        "SymPyを読み込んでいます…";

      status(
        "数学ライブラリを読み込んでいます…"
      );

      try {
        await pyodide.loadPackage("sympy");
      } catch (_) {
        await pyodide.loadPackage("micropip");

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
      $("menuStart").disabled = false;

      $("menuMessage").textContent =
        "準備ができました。ゲームを始めてください。";

      status(
        "メニューからゲームを始めてください。"
      );

    } catch (error) {
      ready = false;
      $("menuStart").disabled = true;

      const message =
        "起動できませんでした：" +
        error.message +
        "\n通信環境を確認して再読み込みしてください。";

      $("menuMessage").textContent = message;
      status(message);
    }
  }

  initialize();
})();
</script>
</body>
</html>
