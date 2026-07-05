# zmk-config-LalaPadGen2

LalaPad Gen2 の ZMK 設定です。

## Hold-tap behaviors

`config/lalapadgen2.keymap` では、hold-tap 用に次の behavior を定義しています。

- `mt_hold`: mod-tap で hold 側を優先したいキー用
- `mt_tap`: mod-tap で tap 側を優先したいキー用
- `mt_hold_on_other`: mod-tap で、単独では tap を待ち、他キーが押された時点で hold にしたいキー用
- `lt_hold_on_other`: layer-tap で、単独では tap を待ち、他キーが押された時点で hold にしたいキー用

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

修飾キーとして使う意図が強いキーに割り当てます。ほかのキーとの同時押しでは hold 側に倒れやすいため、ショートカット用途を優先したい場所に向いています。

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

通常の文字入力で tap 側を優先したいキーに割り当てます。現在の Default layer では、左右の Shift 兼用キーに `z_shift` と `slash_shift`、Command 兼用キーに `d_cmd` と `k_cmd` を使っています。

```dts
&z_shift LEFT_SHIFT Z
&d_cmd LWIN D
&k_cmd RWIN K
&slash_shift RIGHT_SHIFT SLASH
```

これにより、`Z` と `/` は単独押しで文字入力、反対側のアルファキーとの同時押しで Shift として動作します。`D` と `K` も同様に、単独押しで文字入力、同時押しで Command として動作します。

## `mt_hold_on_other` / `lt_hold_on_other` behaviors

`mt_hold_on_other` と `lt_hold_on_other` は Space 系キー用の hold-tap behavior です。`flavor = "hold-preferred"` にして、他キーが押された時点で hold 側に倒れるようにしています。

```dts
mt_hold_on_other: mt_hold_on_other {
    compatible = "zmk,behavior-hold-tap";
    #binding-cells = <2>;
    flavor = "hold-preferred";
    tapping-term-ms = <500>;
    quick-tap-ms = <200>;
    bindings = <&kp>, <&kp>;
};

lt_hold_on_other: lt_hold_on_other {
    compatible = "zmk,behavior-hold-tap";
    #binding-cells = <2>;
    flavor = "hold-preferred";
    tapping-term-ms = <500>;
    quick-tap-ms = <200>;
    bindings = <&mo>, <&kp>;
};
```

Default layer では、2 つの Space 系キーに使っています。

```dts
&lt_hold_on_other COMMAND_LAYER SPACE
&mt_hold_on_other LEFT_CONTROL SPACE
```

`tap-unless-interrupted` は単独押下中に tap 側が先に確定しやすく、Space がキーアップ前に入力されるため使っていません。ここでは `hold-preferred` の tapping term を長めにして、単独の Space はキーアップまで待ちつつ、押している間に別キーが入力された場合はすぐ hold 側に切り替える設定にしています。
