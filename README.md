# zmk-config-LalaPadGen2

LalaPad Gen2 の ZMK 設定です。

## `a_ctrl` behavior

`config/lalapadgen2.keymap` では、Default layer の `A` キーに専用の hold-tap behavior `a_ctrl` を割り当てています。

```dts
&a_ctrl LEFT_CONTROL A
```

目的は、ホームポジションの `A` を単押しでは `A`、他キーとの同時押しでは `Left Ctrl` として使うことです。通常の高速な文字入力で `a` が誤って Ctrl になることを抑えつつ、`Ctrl+Space`、`Ctrl+J`、`Ctrl+K` は成立しやすくしています。

現在の設定は次の通りです。

```dts
a_ctrl: a_ctrl {
    compatible = "zmk,behavior-hold-tap";
    #binding-cells = <2>;
    flavor = "hold-preferred";
    tapping-term-ms = <200>;
    quick-tap-ms = <200>;
    require-prior-idle-ms = <125>;
    hold-trigger-key-positions = <16 17 34>;
    bindings = <&kp>, <&kp>;
};
```

`hold-trigger-key-positions` は、`A` を Ctrl として扱う相手キーを限定しています。

- `16`: `J`
- `17`: `K`
- `34`: 左手側の `Space`

そのため、`a` と他の文字キーを高速にロール入力した場合は tap として扱われやすく、上記のキーと組み合わせた場合だけ hold 側の `Left Ctrl` に倒れます。

調整する場合は、まず `hold-trigger-key-positions` に対象キーを追加または削除します。`a` が Ctrl になりすぎる場合は対象キーを減らし、`Ctrl+Space` などが成立しにくい場合は `tapping-term-ms` を少し伸ばします。

## `r_ctrl` behavior

Default layer の右端 `Enter` キーには、専用の hold-tap behavior `r_ctrl` を割り当てています。

```dts
&r_ctrl RIGHT_CONTROL ENTER
```

目的は、単押しでは `Enter`、`A` または `E` との同時押しでは `Right Ctrl` として素早く反応させることです。`Ctrl+A` と `Ctrl+E` の発火遅延を抑えつつ、他のキーとの組み合わせで Enter が Ctrl になりすぎることを避けています。

現在の設定は次の通りです。

```dts
r_ctrl: r_ctrl {
    compatible = "zmk,behavior-hold-tap";
    #binding-cells = <2>;
    flavor = "hold-preferred";
    tapping-term-ms = <200>;
    quick-tap-ms = <200>;
    require-prior-idle-ms = <125>;
    hold-trigger-key-positions = <2 10>;
    bindings = <&kp>, <&kp>;
};
```

`hold-trigger-key-positions` は、右 `Enter` を Ctrl として扱う相手キーを限定しています。

- `2`: `E`
- `10`: `A`
