---
name: typography-prompt-skill
description: Generate Chinese typography design prompts by matching user text, scene, audience, and reference style against data/style-library.json. Use when the user asks for font design prompts, Chinese title typography, ecommerce campaign lettering, kidswear poster type, brand/logo wordmark typography, Xiaohongshu cover text, or prompts for Image 2.0, Jimeng, or Midjourney.
---

# Typography Prompt Skill

## Skill 作用

根据用户输入的文字、使用场景、参考风格和平台需求，从 `data/style-library.json` 中匹配合适的字体风格，输出可直接用于 Image 2.0、即梦、Midjourney 的中文字体设计提示词。

本 Skill 只生成字体设计策略和提示词，不开发网页、不连接 API、不生成图片、不调用外部服务。

## 适用场景

使用本 Skill 处理以下需求：

- 电商活动标题字：618、双11、年货节、开学季、清仓、直播间、会员日、品牌日。
- 童装儿童字体：可爱、软萌、童趣、亲子、校园、节日、儿童服饰海报。
- 品牌 Logo 字体：中文品牌字、主视觉标题、字标、品牌升级、国潮、轻奢、科技感。
- 小红书封面字：种草、穿搭、清单、教程、生活方式、情绪化标题。
- 任意需要把中文文字转换成字体设计提示词的任务。

## 输入格式

优先要求用户提供以下字段；如果缺失，可根据语义合理补全并说明假设：

```text
文字：要设计的中文标题或 Logo 文案
场景：使用位置或商业场景
参考风格：用户给出的风格词、案例、品牌感或视觉方向
受众：目标人群，可选
平台：Image 2.0 / 即梦 / Midjourney / 全部
比例：如 1:1、3:4、16:9，可选
限制：必须保留的字、禁用元素、品牌色、背景要求，可选
```

简写输入也可以处理，例如：

```text
文字：夏日上新
场景：童装京东活动海报
参考风格：清凉、可爱、冰淇淋色、适合女童
```

## 风格匹配规则

1. 读取 `data/style-library.json`。
2. 根据用户输入提取匹配信号：
   - 场景词：电商、童装、Logo、小红书、节日、促销、科技、国潮、游戏、影视等。
   - 情绪词：高级、可爱、清凉、冲击、复古、温柔、精致、活力、梦幻等。
   - 材质词：金属、塑料、泡泡、毛绒、糖果、玻璃、纸张、刺绣、霓虹等。
   - 色彩词：红金、粉蓝、奶油色、黑金、冰蓝、马卡龙、国潮色等。
   - 结构词：粗黑体、圆润、手写、书法、压缩字、立体字、描边字、扁平字等。
3. 对每个风格进行评分：
   - `scenes` 与场景命中：高权重。
   - `tags` 与参考风格命中：高权重。
   - `name`、`category_name` 与需求类型命中：中高权重。
   - `font_structure`、`material`、`colors` 与风格词命中：中权重。
   - `keywords` 与英文参考词命中：中权重。
4. 选择 1 个主推荐风格；如用户需求混合明显，可补充 1-2 个备选风格，但最终提示词要以主推荐为核心。
5. 如果没有精确命中，选择最接近的类别，并在推荐理由中说明这是相邻匹配。

## 输出格式

每次输出必须包含以下 8 个部分，标题不得省略：

1. 推荐风格
2. 推荐理由
3. 字体结构
4. 材质工艺
5. 色彩建议
6. Image 2.0 提示词
7. 即梦提示词
8. Midjourney 提示词

推荐输出模板：

```markdown
## 1. 推荐风格
风格编号：...
风格名称：...
所属分类：...

## 2. 推荐理由
...

## 3. 字体结构
...

## 4. 材质工艺
...

## 5. 色彩建议
...

## 6. Image 2.0 提示词
...

## 7. 即梦提示词
...

## 8. Midjourney 提示词
...
```

## Prompt 生成规则

所有平台提示词都必须遵守：

- 明确写出用户原始文字，要求只生成该文字。
- 将匹配风格中的 `font_structure`、`material`、`lighting`、`colors` 融合进提示词。
- 强调“中文字体设计、标题字、字形完整、笔画清晰、可读性强”。
- 根据场景补充商业用途，例如“电商主视觉标题”“童装活动海报”“品牌 Logo 字标”“小红书封面标题”。
- 加入排除项：不要多余文字、不要乱码、不要错别字、不要英文替换、不要水印、不要杂乱背景。
- 如果用户指定透明底或纯色底，必须写入提示词。
- 如果用户指定比例，Midjourney 提示词末尾加入 `--ar` 参数。

平台差异：

- Image 2.0：中文描述要完整，强调画面可控、文字准确、字体海报质感。
- 即梦：使用更直接的中文指令，强调“仅保留指定文字”“无其他文字元素”。
- Midjourney：使用中英混合关键词，保留中文文案，追加 typography、Chinese lettering、3D title design、clean composition 等英文关键词。

## 禁止事项

- 不要开发网页应用。
- 不要接入 API。
- 不要编写前端、后端或数据库代码。
- 不要声称已经生成图片。
- 不要输出与用户文字不一致的字体内容。
- 不要建议使用未授权的真实品牌 Logo、受版权保护的字体或在世艺术家的精确风格。
- 不要直接复制参考图中的商标、角色、IP 或完整版式。
- 不要为了追求效果牺牲文字可读性。

## 示例

输入：

```text
文字：夏日上新
场景：童装京东活动海报
参考风格：清凉、可爱、冰淇淋色、适合女童
平台：全部
比例：3:4
```

输出：

```markdown
## 1. 推荐风格
风格编号：优先匹配童装/清凉/可爱相关风格
风格名称：从 style-library.json 读取
所属分类：童装儿童字体或电商活动字体

## 2. 推荐理由
该风格同时命中童装场景、夏季清凉情绪和柔和彩色材质，适合活动海报主标题。

## 3. 字体结构
圆润饱满的中文标题字，笔画偏粗，边角柔和，字形活泼但保持可读。

## 4. 材质工艺
冰淇淋奶油质感、轻微立体厚度、柔和高光、清爽描边。

## 5. 色彩建议
冰蓝、奶油白、浅粉、薄荷绿，可加入少量亮黄色作为点缀。

## 6. Image 2.0 提示词
生成中文字体设计，文字为“夏日上新”，仅出现这四个字，童装活动海报主标题，圆润可爱的粗体字形，清凉夏季氛围，冰淇淋奶油质感，冰蓝、奶油白、浅粉、薄荷绿配色，轻微立体厚度，柔和高光，清爽描边，笔画清晰完整，可读性强，适合京东童装活动主视觉，不要多余文字，不要乱码，不要错别字，不要水印。

## 7. 即梦提示词
请生成一张中文字体标题设计，文字必须是“夏日上新”，画面中只保留这四个字。字体圆润、可爱、饱满，适合童装夏季活动海报；使用冰蓝、奶油白、浅粉、薄荷绿，带冰淇淋奶油质感、轻微立体效果和柔和高光。要求字形完整、笔画清楚、无错别字、无其他文字、无水印。

## 8. Midjourney 提示词
Chinese typography design, exact text "夏日上新", cute rounded bold Chinese lettering, kidswear summer campaign poster title, ice cream cream texture, soft 3D thickness, clean outline, ice blue, cream white, pastel pink, mint green, clear readable strokes, polished commercial title design, no extra text, no typo, no watermark --ar 3:4
```

