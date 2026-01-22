# Rime Mac 使用安装

## 安装rime
```Bash
brew install squirrel-app
```

## 导入导入输入法
- 点击输入法图标
- 打开键盘设置...
- 点击左下角+号 添加简体中文输入法 鼠须管
- 点击完成
- 切换到鼠须管 
- 点击用户设定...
- 开始自定义编辑输入法
- 一般会使用 雾凇拼音进行美化

## 下载雾凇拼音
[](https://github.com/iDvel/rime-ice)

### 采用git 安装 
在用户设定文件夹上一层 删除当前配置(Rime) 然后克隆项目
```Bash
git clone https://github.com/iDvel/rime-ice.git Rime --depth 1

```

## 开始修改个性化配置

修改一些常用配置  
- **default.yaml**

修改个性化界面配置  

- **squirrel.custom.yaml**
下列代码为我的自定义配置样式与微信输入法相仿
```yaml
patch:
  style:
    status_message_type: mix
    # candidate_format: "[label]. [candidate]"
    candidate_format: "%c. %@"       #设置每个候选词之间的间隔距离，%c代表备选的数字，%@代表候选字，可以通过输入空格的形式来调整每个候选字之间的间隔距离
    candidate_list_layout: linear
    text_orientation: horizontal
    #horizontal: true                  #设置水平还是竖直模式
    inline_preedit: true               #设置是否双行显示
    inline_candidate: false
    translucency: false                #半透明
    mutual_exclusive: false
    memorize_size: false
    showPaging: true
    alpha: 1
    corner_radius: 5                  #设置输入条的圆角效果
    hilited_corner_radius: 5          #第一候选字高亮颜色的圆角，当不设置时就是一整块的颜色，设置了圆角之后就带有圆角效果了
    border_height: 0                  #设置输入条上下宽度
    dborder_width: 0                  #设置输入条左右宽度
    border_color: 0x9f62e8            #输入条边框颜色，似乎在横向模式下不起作用
    border_color_width: 0             #输入条边框宽度
    border_width: 0                   #边框宽度
    line_spacing: 0                   #线路间距
    spacing: 0                        #间距
    base_offset: 0                    #基准偏移量
    shadow_size: 0                    #阴影大小
    font_face: PingFangSC-Regular     #字体
    font_point: 16                    #字体大小
    label_font_face: PingFangSC-Light
    label_font_point: 16              #普通标签的字体大小 
    color_scheme: macos_light
    color_scheme_dark: macos_dark

  preset_color_schemes:
    macos_light:
      color_space: display_p3
      back_color: "0xFFFFFFFF"
      hilited_candidate_back_color: "0x6A9956"  #第一候选字高亮颜色（背景色）
      text_color: "0xFFFFFF"                    #普通候选字的颜色，非第一候选字
      candidate_text_color: "0x000000"          #候选字颜色
      hilited_candidate_text_color: "0xFFFFFF"
      label_color: 0x888888                     #普通标签的颜色(非第一候选字)，也就是候选字数字
      hilited_candidate_label_color: "0xFFFFFF" #第一候选字标签颜色，也就是数字1
      hilited_text_color: 0xffffff              #第一候选字颜色

    macos_dark:
      color_space: display_p3
      back_color: "0x000000"
      hilited_candidate_back_color: "0x323232"  #第一候选字高亮颜色（背景色）
      text_color: "0xc8c8c8"                    #普通候选字的颜色，非第一候选字
      candidate_text_color: "0xc8c8c8"          #候选字颜色
      hilited_candidate_text_color: "0xf0f0f0" 
      label_color: 0x888888                     #普通标签的颜色(非第一候选字)，也就是候选字数字
      hilited_candidate_label_color: "0xf0f0f0" #第一候选字标签颜色，也就是数字1
      hilited_text_color: 0xf0f0f0              #第一候选字颜色
```
