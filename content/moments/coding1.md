---
# 置顶 (如需置顶这片Moment, 则将值设定为大于0的整数, 值越小越靠前，例如1会将moment放在最顶端)
top: 

# 名称（不填写则为设置的默认名称）
name: 

# 头像（不填写则为设置的默认头像）
avatar:

# 发布时间
date: 2022-03-13T14:10:39+08:00

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

在 MusicFox 中加入快进功能时，使用了以下代码：

```go
func (p *Player) Speedup() {
	speaker.Lock()
	pos := p.seeker.Position()
	pos += p.SampleRate.N(10 * time.Second)
	err := p.seeker.Seek(pos)
	if err == nil {
		p.timechange += 10 * time.Second
	}
	speaker.Unlock()
}
```

但每次都会`Seek`失败，Debug 发现是获得的`p.seeker.len()=0`，查看 beep 库代码也没有结果，直到追溯回 beep 引用的 go-mp3 ，发现获取时长需要 `io.Seek()`[^1]，显然，读取`request`的`byte reader`没办法完成

[^1]: [源代码见此](https://github.com/hajimehoshi/go-mp3/blob/96c001d18d97b5b09170cab468347a368c3130de/decode.go#L126)

```go
if _, ok := d.source.reader.(io.Seeker); !ok {
	return nil
}
```

我们不得不先把文件下载下来再用`File`读取（笨[^2]）。

[^2]: [这个](https://github.com/jeffallen/seekinghttp/blob/master/seekinghttp.go)应该可以学习，但我太懒了┓( ´∀` )┏