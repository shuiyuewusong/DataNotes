# Rime 小狼毫

## 官网

[](https://rime.im/)

[](https://github.com/rime/home)

## 常用配置

## weasel.custom.yaml
```yaml 
patch:
  # 设置输入法横向显示
  "style/horizontal": true



```


## default.custom.yaml

```yaml
patch:
  # 设置输入法后选词个数
  "menu/page_size": 9 
  ascii_composer/switch_key:
    Caps_Lock: commit_code #Caps键
    Shift_L: noop #左Shift，切换中英文
    Shift_R: noop #右Shift，切换中英文
    Control_L: noop #左Control，屏蔽该切换键
    Control_R: noop #右Control，屏蔽该切换键

```

