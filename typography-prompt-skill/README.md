<p align="center">
  <img src="[你复制的Raw图片链接](https://raw.githubusercontent.com/smaball117/typography-prompt-skill/refs/heads/main/typography-prompt-skill/assets/cover.png)" width="100%">
</p>
# Typography Prompt Skill

`typography-prompt-skill` 是一个供 Codex 调用的字体提示词 Skill。它根据用户输入的文字、场景和参考风格，从 `data/style-library.json` 中匹配合适的字体风格，并输出可直接用于 Image 2.0、即梦、Midjourney 的字体设计提示词。

## 文件结构

```text
typography-prompt-skill/
├─ SKILL.md
├─ README.md
├─ data/
│  └─ style-library.json
└─ examples/
   ├─ ecommerce.md
   ├─ kidswear.md
   ├─ logo.md
   └─ xiaohongshu.md
```

## 使用方式

当用户需要生成中文字体设计提示词时，读取 `SKILL.md` 的流程说明，并从 `data/style-library.json` 中匹配风格。

推荐输入：

```text
文字：春夏上新
场景：童装电商活动海报
参考风格：清新、软萌、明亮、适合儿童服饰
平台：全部
比例：3:4
```

固定输出包含：

1. 推荐风格
2. 推荐理由
3. 字体结构
4. 材质工艺
5. 色彩建议
6. Image 2.0 提示词
7. 即梦提示词
8. Midjourney 提示词

## 注意

这是纯 Skill 文档和数据结构，不包含网页应用、API 接入、服务端代码或图片生成逻辑。

