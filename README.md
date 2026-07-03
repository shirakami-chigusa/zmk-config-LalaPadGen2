# zmk-config-LalaPadGen2

LalaPad Gen2 の ZMK 設定です。

## Mod-tap behaviors

`config/lalapadgen2.keymap` では、mod-tap 用に次の 2 種類の hold-tap behavior を定義しています。

- `mt_hold`: hold 側を優先したいキー用
- `mt_tap`: tap 側を優先したいキー用

どちらも `tapping-term-ms = <200>`、`quick-tap-ms = <200>`、`require-prior-idle-ms = <125>` を使います。

## `mt_hold` behavior

`mt_hold` は `flavor = "hold-preferred"` の汎用 mod-tap behavior です。

```dts
mt_hold: mt_hold {
    compatible = "zmk,behavior-hold-tap";
    #binding-cells = <2>;
    flavor = "hold-preferred";
    tapping-term-ms = <200>;
    quick-tap-ms = <200>;
    require-prior-idle-ms = <125>;
    bindings = <&kp>, <&kp>;
};
```

修飾キーとして使う意図が強いキーに割り当てます。ほかのキーとの同時押しでは hold 側に倒れやすいため、Ctrl や Space/Backspace 兼用キーなど、ショートカット用途を優先したい場所に向いています。

## `mt_tap` behavior

`mt_tap` は `flavor = "tap-preferred"` の汎用 mod-tap behavior です。

```dts
mt_tap: mt_tap {
    compatible = "zmk,behavior-hold-tap";
    #binding-cells = <2>;
    flavor = "tap-preferred";
    tapping-term-ms = <200>;
    quick-tap-ms = <200>;
    require-prior-idle-ms = <125>;
    bindings = <&kp>, <&kp>;
};
```

通常の文字入力で tap 側を優先したいキーに割り当てます。Default layer では左右の Shift 兼用キーに使っています。

```dts
&mt_tap LEFT_SHIFT Z
&mt_tap RIGHT_SHIFT MINUS
```

これにより、`z y` のように素早くロール入力したときに、`Z` が Shift として扱われて `Y` になる誤爆を抑えます。右側も同様に、`-` の直後に別キーを入力したときの Shift 誤爆を抑えます。

調整する場合は、まず `mt_tap` の `tapping-term-ms` を短くすると tap 判定が確定しやすくなります。Shift として成立しにくい場合は `tapping-term-ms` を少し伸ばします。
