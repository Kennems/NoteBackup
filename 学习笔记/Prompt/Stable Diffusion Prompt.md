---
title : 'Stable Diffusion Prompt'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "Stable Diffusion（稳定扩散模型）是一种基于潜在扩散模型（Latent Diffusion Model）的文本生成图像 AI。它通过在大规模图文数..."
image : img/cat.jpg
draft : false
categories : ["Prompt"]
tags : ["学习笔记", "Prompt"]
---
# Stable Diffusion Prompt

## 一、Stable Diffusion 与提示词工作原理

Stable Diffusion（稳定扩散模型）是一种基于潜在扩散模型（Latent Diffusion Model）的文本生成图像 AI。它通过在大规模图文数据集上训练，学会了将文本描述与视觉特征关联起来。

提示词（Prompt）是用户输入的自然语言文本，模型通过 CLIP（Contrastive Language-Image Pre-training）文本编码器将提示词转换为向量表示（embedding），然后扩散模型以这个向量为条件，从随机噪声开始逐步去噪，最终生成符合描述的图像。

提示词的质量直接影响生成结果。好的提示词需要精确描述主体、环境、风格和画质，同时配合负面提示词排除不想要的元素。

## 二、提示词结构

### 2.1 基本组成

一个完整的提示词通常包含以下几个维度：

- **主体（Subject）**：核心描述对象，如 "a beautiful woman"、"a cat"、"a futuristic city"
- **风格（Style）**：艺术风格或流派，如 "oil painting"、"anime"、"photorealistic"、"pixel art"
- **质量（Quality）**：画质修饰词，如 "masterpiece"、"best quality"、"highly detailed"、"8K"
- **光线（Lighting）**：光照条件，如 "cinematic lighting"、"warm lighting"、"volumetric light"
- **构图（Composition）**：镜头视角和构图方式，如 "full body shot"、"close-up"、"dynamic angle"
- **颜色（Color）**：色调描述，如 "vivid colors"、"pastel tones"、"monochrome"
- **背景（Background）**：环境描述，如 "night view of skyscrapers"、"forest clearing"、"space"

### 2.2 提示词撰写原则

- **描述具体**：避免模糊词汇，使用 precise 的描述
- **英文优先**：SD 训练数据以英文为主，中文支持不如英文稳定
- **关键词在前**：模型更关注句子前半部分的词汇
- **逗号分隔**：不同概念用逗号隔开，每个逗号相当于一个分隔标记
- **合理长度**：CLIP 模型最多支持 77 个 token（约 50-60 个英文单词），超出部分会被截断

### 2.3 Prompt 模板

```
[质量词], [主体描述], [细节特征], [服装/配饰], [姿势], [背景], [光线], [构图], [风格]
```

## 三、Token 权重

### 3.1 语法格式

| 语法 | 效果 | 说明 |
|------|------|------|
| `(keyword)` | 增强 1.1 倍 | 圆括号提高注意力 |
| `((keyword))` | 增强 1.21 倍 | 双重括号叠加 |
| `(((keyword)))` | 增强 1.33 倍 | 三重括号叠加 |
| `(keyword:1.5)` | 增强 1.5 倍 | 精确指定权重 |
| `(keyword:0.8)` | 减弱 0.8 倍 | 降低关注度 |
| `[keyword]` | 减弱 1.1 倍 | 方括号降低注意力 |
| `{keyword}` | 增强 1.05 倍 | 大括号（NovelAI 语法） |

### 3.2 权重使用技巧

- **关键特征加重**：对核心特征如面部细节、眼睛等加重（如 `(detailed eyes:1.5)`）
- **非必要元素减轻**：对背景等次要元素降低权重
- **权重不宜过高**：超过 1.8 可能导致图像过拟合或 artifact
- **叠加使用**：`((masterpiece:1.4))` 表示同时应用圆括号和权重值
- **权重范围建议**：推荐在 0.5-1.8 之间，极端值可能导致异常结果

### 3.3 BLIP/WD14 标签权重

在训练 embeddings 或 LoRA 时，常常使用 BLIP 或 WD14 Tagger 生成的自然语言标签作为提示词，这些标签通常带有置信度权重，可在训练时用于加权损失函数。

### 3.4 权重衰减策略

当我们使用 `(keyword):1.5` 时，模型会对该词的语义概念赋予 1.5 倍的交叉注意力分数。但同义或近义词之间存在竞争，过多的关键字可能导致注意力稀释。因此建议在同一概念上使用 2-3 个近义词（如 `(masterpiece, best quality, highres)`）而非堆叠同一种表述。

## 四、负面提示词（Negative Prompt）

### 4.1 概念

负面提示词是告知模型**不要生成**的内容列表。它通过将不希望的特征加入文本编码条件中，在去噪过程中引导模型远离这些特征。

### 4.2 核心分类

| 类别 | 示例 |
|------|------|
| 画质缺陷 | `worst quality`, `low quality`, `blurry`, `jpeg artifacts` |
| 解剖问题 | `bad anatomy`, `bad hands`, `missing fingers`, `extra limbs`, `mutated hands` |
| 比例失调 | `bad proportions`, `gross proportions`, `malformed limbs` |
| 多余元素 | `text`, `watermark`, `signature`, `username`, `artist name` |
| 颜色问题 | `monochrome`, `grayscale` |
| 透视问题 | `cropped`, `tilted head`, `facing away` |
| 年龄体型 | `child`, `loli`, `petite`, `muscular` (根据需求) |
| 特定风格 | `sketches`, `3D face`, `nsfw` (视需求) |

### 4.3 高效负面提示词用法

- **使用 Embedding 组合**：像 `verybadimagenegative_v1.3`、`ng_deepnegative_v1_75t` 这类负面 embedding 是经过专门训练的，比手写列表效果更好
- **权重控制**：对严重的问题使用更高权重如 `(bad anatomy:1.3)`，对边缘问题使用 `0.8-1.0`
- **避免过度限制**：过多的负面词可能导致生成结果僵硬或内容缺失
- **版本匹配**：不同 SD 版本（1.5 / 2.1 / XL）的负面提示词效果不同，SDXL 需要更少的负面词
- **特异性**：负面提示词越具体越好，如 `(wrong fingers:1.2)` 比直接写 `hands` 更有效

## 五、常见提示词分类

### 5.1 人像/人物摄影

核心要素：面容细节、发型、服装、姿势、表情、皮肤质感、眼部高光

```
(masterpiece:1.4), portrait of a 25-year-old Asian woman, detailed face, (intricate eyes:1.3), soft smile, flowy brown hair, (skin texture:1.2), bokeh background, cinematic lighting, 85mm lens, f/1.8
```

### 5.2 风景/环境

核心要素：自然元素、天气、光照、视角、景深、季节

```
breathtaking landscape, (mountain range:1.3), morning fog, golden hour, (reflective lake:1.2), detailed foliage, volumetric lighting, wide angle, photorealistic
```

### 5.3 奇幻/科幻

核心要素：超自然元素、魔法光线、未来感建筑、异世界风格

```
(fantasy world:1.3), ethereal cathedral, floating islands, magical glowing crystals, dramatic sky, epic scale, intricate architecture, surreal atmosphere, octane render
```

### 5.4 概念艺术/插画

核心要素：画风、笔触、构图、色彩方案、艺术流派

```
concept art, (digital painting:1.2), Greg Rutkowski style, splash art, vibrant color palette, strong composition, detailed brushwork, atmospheric
```

### 5.5 产品/静物

核心要素：材质、反光、纹理、布局、景深

```
product photography, luxury perfume bottle, (glass reflections:1.3), macro shot, premium lighting, clean background, sharp focus, commercial photography, 4K
```

## 六、进阶技巧

### 6.1 CFG Scale（无分类器引导尺度）

- **定义**：控制提示词对生成图像的引导强度
- **范围**：1.0 - 30.0（默认 7.0）
- **低值（1-5）**：生成的图像与提示词关联弱，但更自然、自由度更高
- **中值（7-12）**：平衡提示词遵循度和图像质量，最常用范围
- **高值（15-30）**：严格遵循提示词，但可能导致过饱和、artifact、色彩失真
- **建议**：SD 1.5 推荐 7-10；SDXL 推荐 5-8；追求创意可选 4-6

CFG Scale 与权重的关系：CFG Scale 是全局引导力度，而权重是局部调整。两者结合时建议 CFG 在 7-9 范围内，权重不超过 1.6，否则容易产生过饱和。

### 6.2 Seed（随机种子）

- **定义**：决定初始噪声图的随机数
- **作用**：相同 prompt + 相同 seed 可复现相同图像
- **-1**：每次生成使用随机种子
- **固定种子**：对同一 prompt 微调时可固定 seed，方便对比效果
- **种子探索**：使用不同的种子可以获得同一提示词的不同构图变体
- **种子变异**：在种子值上做小幅修改（±1）会产生视觉关联但不完全相同的图像

### 6.3 Sampler（采样器）

| 采样器 | 特点 | 推荐步数 |
|--------|------|----------|
| Euler | 速度快，结果稳定 | 20-30 |
| Euler a | 更生动的结果，有轻微变化 | 20-30 |
| DPM++ 2M Karras | 高质量，细节丰富 | 20-30 |
| DPM++ SDE Karras | 细节极佳，稍慢 | 25-35 |
| DDIM | 经典采样器，可少步数生成 | 10-20 |
| PLMS | 旧版采样器，不推荐 | - |
| LCM/Lightning | 极速采样，4-8 步即可 | 4-8 |

**建议**：通用推荐 DPM++ 2M Karras（20-25 步）；质量优先选 DPM++ SDE Karras（30 步）；速度优先选 Euler（20 步）。

采样器与步数的配合：步数过少（<15）细节不足，步数过多（>40）边际收益递减且可能引入噪点。最佳步数取决于采样器类型，通常 20-30 步是经济高效的选择。

### 6.4 Denoising Strength（去噪强度）

用于 img2img 时，控制原图保留比例：
- **0.0**：完全保留原图
- **0.3-0.5**：轻量修改（修图、调色）
- **0.5-0.7**：大幅修改（风格迁移）
- **0.7-1.0**：几乎完全重绘

### 6.5 High-Res Fix（高清修复）

原理：先生成低分辨率图像（如 512×512），再以 img2img 方式放大到更高分辨率（如 1024×1024）。

参数建议：
- 放大倍数：1.5-2 倍
- 去噪强度：0.3-0.5
- 放大算法：ESRGAN_4x / R-ESRGAN 4x+ / SwinIR

## 七、Embeddings 与 LoRA

### 7.1 Textual Inversion（Embedding）

- **原理**：将特定概念（如人脸、风格、物体）训练成小型向量文件（通常 5-100 KB）
- **使用方式**：在提示词中直接引用文件名，如 `(my_character_embedding:0.8)`
- **训练数据**：3-15 张图片即可
- **权重控制**：通过括号调整影响力，一般 0.6-1.2
- **优点**：文件小、不修改模型、易共享
- **缺点**：表达能力有限，复杂概念效果一般
- **常见公共 Embedding**：`easynegative`、`bad-hands-5`、`verybadimagenegative_v1.3`

### 7.2 LoRA（Low-Rank Adaptation）

- **原理**：在模型特定层上添加低秩适配矩阵（通常 5-200 MB），改变模型的输出风格或人物特征
- **使用方式**：UI 中加载 LoRA 文件，设置权重比例
- **权重控制**：通常 0.3-1.5（取决于训练强度）
  - 0.3-0.6：轻微影响
  - 0.7-1.0：适中效果（推荐）
  - 1.2-1.5：强效果（可能过拟合）
- **常见类型**：
  - 角色 LoRA（特定人物外观）
  - 服装 LoRA（特定服饰风格）
  - 姿势 LoRA（特定动作）
  - 概念 LoRA（特定主题或物体）
  - 风格 LoRA（特定画风）
- **触发词**：部分 LoRA 需要特定触发词才能激活，作者会在模型说明中提供
- **多 LoRA 叠加**：可同时使用多个 LoRA（如角色 + 服装），但权重不宜过高，总建议 ≤1.5

### 7.3 LyCORIS

LoRA 的变体，使用更复杂的矩阵分解方式，在相同参数量下有更强的表达能力。通常使用较低的权重（0.4-0.8）即可达到明显效果。

### 7.4 Embedding 与 LoRA 的搭配

```
(masterpiece:1.4), <lora:my_style_lora:0.8>, (my_face_embedding:1.0), portrait of a woman, detailed face, cinematic lighting
```

在提示词中结合使用 embedding 和 LoRA 可以获得复合效果，但需要注意权重平衡，避免两者冲突导致特征混乱。

## 八、获得一致性结果的技巧

### 8.1 基础知识

- **固定种子**：使用同一 seed 和同一 prompt 可复现相同图像
- **记录参数**：保存 CFG Scale、Sampler、Steps 等参数组合
- **使用模板**：建立自己的 prompt 模板，只替换核心词

### 8.2 参数调优策略

当结果不理想时，建议逐一排查：

1. 检查负面提示词是否过度限制
2. 降低 CFG Scale 看是否过饱和
3. 增加采样步数看是否细节不足
4. 调整描述词的顺序（重要词提前）
5. 检查 embedding/LoRA 权重是否过大
6. 更换采样器类型

### 8.3 人物一致性

- **人物 LoRA**：训练特定角色的 LoRA 是保持人脸的可靠方法
- **Seed 复用**：对同一人物描述使用固定种子微调
- **ControlNet**：使用 Canny / OpenPose / Depth 等 ControlNet 模型控制结构和姿态
- **IP-Adapter**：参考图保持风格，适合 SDXL
- **表情差异**：在同一 seed 下修改表情相关词（如 `gentle smile` → `serious expression`）可改表情而不改变整体构图

### 8.4 实用技巧

- **反向推理**：如生成的图像有新元素，可将其加入负面提示词移除
- **分步构建**：先确定主体构图，再逐步添加细节和风格
- **批量生成**：同一 prompt 用多个 seed 生成，选择最佳结果
- **图片信息（PNG Info）**：在 WebUI 中将生成的图片拖入 PNG Info 选项卡，可查看完整生成参数

### 8.5 常见问题排查表

| 问题 | 可能原因 | 解决方法 |
|------|----------|----------|
| 多人/多余人物 | 提示词歧义 | 明确 `1girl` / `solo`，或加入 `(multiple people:0.5)` 到负面 |
| 面部变形 | 权重不足或步数不够 | 增加 `(detailed face:1.5)`，提高步数 |
| 色彩过饱和 | CFG 过高 | 降低 CFG Scale 至 5-7 |
| 细节不足 | 步数太少或权重低 | 增加步数至 25-30，增加质量词 |
| 手指畸形 | 模型固有缺陷 | 使用负面 embedding `bad-hands-5` 或 LoRA `hands_lora` |
| 构图雷同 | seed 锁定 | 更换 seed 或用不同采样器 |

## 九、完整示例

### 示例：人物肖像提示词

```
(((masterpiece:1.4, Best Quality:1.4, 8K, ))), ultra high res, Physically Based Rendering, Portrait, ultra Beautiful Sexy Woman, whole body, (blemish-free skin), (supermodel body:1.8), (a beautiful 20-year-old cute Korean woman: 1.1), (kpop idol, Korean mixed), Detailed large eyes, detailed thin eyebrows, Highlights in the eyes, short half-updo light brown hair, ((strong wind blows her hair:1.2)), Highly detailed facial and skin texture, Crisp focus, Clear focus:1.2, Slender beauty:1.4, ((huge breasts:1.5)), erect nipples, Slim abs, oval-face, Lip gloss, Clear facial features, gentle smile, bare shoulders glossy pink sexy tight dress of metallic violet, beauty legs, full body shot, standing, warped waist, night view of skyscrapers:1.4, warm lighting, (full wide shot:1.8), (random pose), official art, unity 8k wallpaper, ultra detailed, beautiful and aesthetic, masterpiece, best quality, (zentangle, mandala, tangle, entangle), (fractal art:1.3), 1girl, big breast, extremely detailed, dynamic angle, cowboyshot, the most beautiful form of chaos, elegant, a brutalist designed, vivid colours, romanticism, atmospheric, ecstasy of musical notes, streaming musical notes visible, physically-based rendering, (detailed beautiful eyes and detailed face:1.5), (focus on face:1.3), (majestic fantasy:1.2), (huge breasts hidden under clothes:1.4), pureerosface_v1, (otherworldly), highly insanely detailed, masterpiece, top quality, best quality, highres, 4k, 8k, RAW photo, 1girl, rice, (fantasy world), chest point, intricate detail face, cinematic light, ultra high res, break, 1girl, stuffed toy, stuffed animal, brown hair, solo, shirt, collar, ponytail, indoors, teddy bear, brown eyes, pantyhose, cat
```

### 示例：负面提示词

```
verybadimagenegative_v1.3, ng_deepnegative_v1_75t, (ugly face:0.8), cross-eyed, sketches, (worst quality:2), (low quality:2), (normal quality:2), lowres, normal quality, ((monochrome)), ((grayscale)), skin spots, acnes, skin blemishes, bad anatomy, DeepNegative, facing away, tilted head, {Multiple people}, lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worstquality, low quality, normal quality, jpegartifacts, signature, watermark, username, blurry, bad feet, cropped, poorly drawn hands, poorly drawn face, mutation, deformed, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, extra fingers, fewer digits, extra limbs, extra arms, extra legs, malformed limbs, fused fingers, too many fingers, long neck, cross-eyed, mutated hands, polar lowres, bad body, bad proportions, gross proportions, text, error, missing fingers, missing arms, missing legs, extra digit, extra arms, extra leg, extra foot, ((repeating hair)), (worst quality, low quality:1.5), (depth of field, blurry:1.2), (greyscale, monochrome:1.1), 3D face, nose, cropped, lowres, text, jpeg artifacts, signature, watermark, username, blurry, artist name, trademark, title, (tan, muscular, loli, petite, child, infant, toddlers, chibi, sd character:1.1), multiple view, Reference sheet, bag, sheer, (bad anatomy:1.3), (wrong legs:1.8), (wrong arms:1.1), (wrong body:1.3), (wrong body:1.3), (High leg:1.5), (wrong hands), (wrong foot:1.4), trees, buildings, wrong body length, complicated background, fluttering hem of clothes, (body structure that cannot be identified), (body fusion:1.1), (non dynamic beautiful posed), (non smile girl:1.4), long cloak, cumbersome fabric, cumbersome floating object, (muscle:1.8), (watermark), (shit belly), chiffon, easynegative
```

Seed: 2237040078

## 十、模型版本差异

### SD 1.5 vs SDXL

| 维度 | SD 1.5 | SDXL |
|------|--------|------|
| 基础分辨率 | 512×512 | 1024×1024 |
| 提示词敏感性 | 高，需精确权重 | 略低，更理解自然语言 |
| 负面提示词 | 必须，大量使用 | 可精简，少量即可 |
| CFG Scale | 7-10 | 5-8 |
| 采样步数 | 20-30 | 20-40 |
| 风格控制 | 依赖权重 | 原生理解更好 |
| 推荐 Sampler | DPM++ 2M Karras | DPM++ 2M Karras / DDIM |

SDXL 对自然语言的理解能力更强，不再需要大量"标签式"关键词。相反，一段通顺的英文描述在 SDXL 中往往比 SD 1.5 产生更好的效果。建议 SDXL 用户减少负面提示词量，降低权重值，使用更自然的描述方式。

## 十一、推荐资源

- **Prompt 分享社区**：CivitAI (civitai.com)、Hugging Face (huggingface.co)
- **Embedding 下载**：CivitAI、Hugging Face 的 Textual Inversion 分类
- **LoRA 模型**：CivitAI 是最大 LoRA 资源库
- **在线测试**：Hugging Face 的 Stable Diffusion WebUI (免费试用)
- **参数查询**：SD WebUI 的 PNG Info 功能可查看他人出图的所有参数
- **官方文档**：CompVis/stable-diffusion GitHub 仓库
