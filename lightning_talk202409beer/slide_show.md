---
marp: true
paginate: true
---

# Lightning Talk

【交流会】SKYWILL Beer Bash 9月

![bg right](./Oktoberfest.avif)

---

# 自己紹介

- 名前：平良一真
- 所属：ASD推進室

---

# ところでみなさん最近どこで飲みました？

---

![bg 60%](./saizeriya.png)

---

# サイゼ飲みと言えば・・・

---

# マグナム！

![bg 35%](./magnum.png)

---

# 青豆！

![bg 60%](./greenpeas.png)

---

# そして何より・・・

---

# <!--fit--> 間違い探し！

---

# 激ムズのこれ

![100%](./spot.png)

---

# 今日は「ITに関すること」がテーマということで

---

## 楽しみながらJSONの仕様について学べる

# <!--fit-->JSON間違い探し

---

# 例題その1

```JSON
{
  "key": "value"
}
```

↑これはJSONとして正しい

# 例題その2

```JSON
{
  "key": "value"

```

↑これは `}` （閉じカッコ）がないのでJSONとして間違い

---

# 第一問

```JSON
false
```

---

<div style="text-align: center;font-size: 500px">
🙆‍♂️
</div>

---

# 第一問解説

> 2. JSON Grammar
>
> ```
> JSON-text = ws value ws
> 
> ws = *(
>     %x20 /    ; Space
>     %x09 /    ; Horizontal tab
>     %x0A /    ; Line feed or New line
>     %x0D )    ; Carriage return
> ```

> 3. Values
>
> ```
> value = false / null / true / object / array / number / string
> false = %x66.61.6c.73.65   ; false
> ```

---

# 第一問解説

- JSONはobjectに限らず値であればOK
- ただし、廃止された旧仕様の `RFC 4627` ではobjectまたはarrayに限られていた

> A JSON text is a serialized object or array.
> JSON-text = object / array

---

# 第二問

```JSON
{
  "name": {
    "jp": "株式会社 スカイウイル",
    "en": "Skywill inc.",
  },
  "website": "https://www.skywill.jp",
  "address": "東京都品川区北品川5-9-11 大崎MTビル10F",
  "tel": "03-5449-6090",
  "fax": "03-3447-3305",
}
```

---

<div style="text-align: center;font-size: 500px">
🙅🙅‍♀️
</div>

---

# 第二問解説 その1

> 2. JSON Grammar
>
> ```
> begin-object    = ws %x7B ws  ; { left curly bracket
> end-object      = ws %x7D ws  ; } right curly bracket
> name-separator  = ws %x3A ws  ; : colon
> value-separator = ws %x2C ws  ; , comma
> ```

> 4. Objects
>
> ```
> object = begin-object [ member *( value-separator member ) ] end-object
> member = string name-separator value
> ```

---

# 第二問解説 その1

- JSONのobjectやarrayでは、末尾の要素に `,` （カンマ）は認められていない

```JSON
{
  "name": {
    "jp": "株式会社 スカイウイル",
    "en": "Skywill inc."「,」 ←これと
  },
  "website": "https://www.skywill.jp",
  "address": "東京都品川区北品川5-9-11 大崎MTビル10F",
  "tel": "03-5449-6090",
  "fax": "03-3447-3305"「,」 ←これ
}
```

---

# 第二問解説 その2

> 7. Strings
>
> ```
> string = quotation-mark *char quotation-mark
>
> char = unescaped /
>     escape (
>         %x22 /          ; "    quotation mark  U+0022
>         %x5C /          ; \    reverse solidus U+005C
>         %x2F /          ; /    solidus         U+002F
>         %x62 /          ; b    backspace       U+0008
>         %x66 /          ; f    form feed       U+000C
>         %x6E /          ; n    line feed       U+000A
>         %x72 /          ; r    carriage return U+000D
>         %x74 /          ; t    tab             U+0009
>         %x75 4HEXDIG )  ; uXXXX                U+XXXX
> escape = %x5C              ; \
> quotation-mark = %x22      ; "
> unescaped = %x20-21 / %x23-5B / %x5D-10FFFF
> ```

---

# 第二問解説 その2

- JSONの文字列では単独 `/` は認められておらず、 `\` でのエスケープが必要

```JSON
{
  "name": {
    "jp": "株式会社 スカイウイル",
    "en": "Skywill inc."
  },
  "website": "https:\/\/www.skywill.jp", ←エスケープする
  "address": "東京都品川区北品川5-9-11 大崎MTビル10F",
  "tel": "03-5449-6090",
  "fax": "03-3447-3305"
}
```

---

# 第三問

```JSON
{
  "hoge": 1,
  "hoge": 2,
  "hoge": 3
}
```
---

<div style="text-align: center;font-size: 500px">
🙆‍♀️
</div>

---

# 第三問解説

> 4. Objects
>
> ...The names within an object SHOULD be unique.
> ...When the names within an object are not unique, the behavior of software that receives such an object is unpredictable.

- objectのキーは重複しないことが推奨されているが重複しても文法の誤りではない
- ただし、そのようなJSONを受けとったソフトウェア上では未定義動作となる

---

# 第四問

```JSON
{
  "big number": 2147483648,
  "huge number": 9007199254740992,
  "very huge number": 9223372036854775808,
  "super huge number": 1.7976931348623157E309
}
```

注）
- 32bit整数の最大値は2147483647
- 53bit整数の最大値は9007199254740991
- 64bit整数の最大値は9223372036854775807
- 倍精度浮動小数点数の最大値は1.7976931348623157e+308

---

<div style="text-align: center;font-size: 500px">
🙆
</div>

---

# 第四問解説

> 6. Numbers
>
> ```
> number = [ minus ] int [ frac ] [ exp ]
>
> decimal-point = %x2E       ; .
> digit1-9 = %x31-39         ; 1-9
> e = %x65 / %x45            ; e E
> exp = e [ minus / plus ] 1*DIGIT
> frac = decimal-point 1*DIGIT
> int = zero / ( digit1-9 *DIGIT )
> minus = %x2D               ; -
> plus = %x2B                ; +
> zero = %x30                ; 0
> ```

---

# 第四問解説

- JSONのnumber型は十進数が表せる
  - 整数・小数に加えて指数表記も可能
- 数値の範囲の制限は無し
  - JSONはあくまでデータの表記法に過ぎないので、それをメモリ上でどのように配置するか・どのような範囲や精度の数値を扱うかは考慮されていない
  - ただし、JSONを扱うソフトウェア上では数値の範囲と精度を制限可能

---

# 第五問

```JSON
{
  "key": "value"　
}
```

---

<div style="text-align: center;font-size: 500px">
🙅‍♂️
</div>

---

# <!--fit-->全角スペース

---

# <!--fit-->ダメ。ゼッタイ。

---

# 第五問解説

```JSON
{
  "key": "value"「　」←ここにいる
}
```

---

# ご清聴ありがとうございました。
