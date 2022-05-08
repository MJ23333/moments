---
# 置顶 (如需置顶这片Moment, 则将值设定为大于0的整数, 值越小越靠前，例如1会将moment放在最顶端)
top: 

# 名称（不填写则为设置的默认名称）
name: 

# 头像（不填写则为设置的默认头像）
avatar:

# 发布时间
date: 2022-03-14T11:33:23+08:00

# 给Moment添加标签
tags:
 -
 -

# 附加信息（选填1项或者不填写）

# 附加信息类型1:单个图片
pictures:
-

# 附加信息类型2:网页链接
# link：必填，网页链接；
# link_text：必填，链接显示的文字；
# link_logo：选填，网页logo，现在支持部分网站自动找到对应图标，无需自己添加图标
link:
link_text:
link_logo:

# 备注信息
note:
---

制作德狗桌宠的时候需要在Mac上得到一个透明窗口，MJ首先用了以下方法：

```objc
NSWindow *cocoa_window = (NSWindow*)window.getSystemHandle() ;
GLint opaque = 0;
[cocoa_window setStyleMask:NSWindowStyleMaskBorderless];
[cocoa_window setLevel: NSFloatingWindowLevel];
[[[cocoa_window contentView] openGLContext] setValues:&opaque forParameter:NSOpenGLContextParameterSurfaceOpacity];
[cocoa_window setBackgroundColor:[NSColor clearColor]];
[cocoa_window setOpaque:NO];
```

但鼠标不能穿过透明部分，读 Apple 的文档试了 1h 后没有任何改善。不得不跑到 Stack Overflow 问出了人生中第一个问题。

根据我的经验，应该没有人会回答，😞。

---

补充：果然没人回答（然而在 Chemistry StacOverflow 上的问题被秒回了，还得到了极好的文献）

最后的方法是直接放弃让WindowManager处理所有鼠标事件，让 SFML 检测鼠标按下的位置后人工判定是否戳中德狗（笨）。