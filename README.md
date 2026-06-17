# World Cup Opossum Pet Skill

> 把世界杯赛程、球队风格和打工人看球状态，变成一只可安装、可二创的 Codex 负鼠宠物。
>
> 固定负鼠本体 | 动态多帧宠物 | 可改球衣/文案/赛程牌 | 9 个 Codex 状态 | 中国时间赛程

## 状态预览

![背手球评负鼠 Codex 状态对应图](examples/images/current-state-map.png)

## 这个仓库是什么

这是一个面向 REDSkill / Codex Skill 的世界杯宠物模板。它不是通用生图 prompt，也不是单张表情包生成器，而是一个可以指导 Codex 生成、配置、安装和迭代“背手球评负鼠”的动态宠物 Skill。

重点：它的真正产出是 **Codex 动态宠物 spritesheet**。状态对应图只是展示物料，不是最终产品本体。

核心设定是：负鼠本体固定，用户只改外层配置。

可改内容：

- 宠物名字
- 状态文案
- 球队风格
- 球衣对应状态
- 是否显示赛程
- 宠物大小和清晰度
- 中国时间赛程牌样式

不可默认改动：

- 负鼠主体
- 侧脸、背手、冷静疲惫气质
- 9 个 Codex 状态契约和动态多帧循环

## 和 Ian Xiaohei Illustrations 的关系

形态上类似：都是“固定 IP + Skill 规则 + 示例/参考 + 可复用产出”的仓库。

区别是：

- Ian Xiaohei Illustrations 产出正文配图和 shot list。
- World Cup Opossum Pet Skill 产出 Codex 动态宠物包、状态配置和赛程牌 spritesheet。
- Ian 的自由度在“为文章重新发明隐喻”。
- 负鼠 Skill 的自由度在“换球队风格、状态文案和赛程牌样式”，负鼠主体要锁住。

## 默认产出

- 一个可安装的 Codex pet 文件夹
- `pet.json`
- `spritesheet-daily-board.png`，8 列 x 9 行动态图集
- `pet.config.json`
- 当前状态对应图，用于展示和返稿说明
- 黄色 waiting/waving 完整耳朵参考图
- review 默认使用裁判风，避免和阿根廷 idle/running 混淆
- 可选：小红书发布素材图

动态规则：

- 每一行对应一个 Codex 状态。
- 每个状态有 4-8 帧循环动画。
- Codex 运行时会根据状态切换不同动画行。
- `running-right` 和 `running-left` 必须是方向明确的动态移动状态。
- `idle / waiting / running / review / failed / waving / jumping` 都不能只做成静态截图。

## 安装形态

真正需要复制到 Codex skills 目录的是子目录：

```text
worldcup-opossum-pet/
```

示例：

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R ./worldcup-opossum-pet "${CODEX_HOME:-$HOME/.codex}/skills/"
```

安装后在 Codex 里使用：

```text
Use $worldcup-opossum-pet 安装世界杯负鼠宠物。
```

注意：这个 Skill 依赖本地已安装的 `$hatch-pet`。它负责世界杯负鼠的配置、文案和主题规则；真正的动态宠物图集生成、校验和安装由 `$hatch-pet` 接管。

安全边界：下载 Skill 本身不会自动接管 Codex 宠物；只有用户在 Codex 里明确运行安装请求时，才会通过 `$hatch-pet` 写入 `${CODEX_HOME:-$HOME/.codex}/pets/<pet-id>/`。配置文件只当数据读取，不执行其中的内容。

换句话说：

- 安装 Skill：只是让 Codex 认识 `$worldcup-opossum-pet`。
- 运行安装请求：才会通过 `$hatch-pet` 生成/安装宠物包。
- 显示宠物：依赖 Codex 宠物系统读取 `${CODEX_HOME:-$HOME/.codex}/pets/<pet-id>/pet.json` 和 spritesheet；如果 Codex 有缓存，可能需要刷新、重启或切换宠物。
- 赛程牌：是构建时写进 spritesheet 的画面，不是自动联网刷新的实时组件；要保证每天准确，需要单独刷新 fixture 数据并重建/重装宠物。

或：

```text
Use $worldcup-opossum-pet 把我的主队改成巴西风，保留赛程牌，文案更阴阳怪气一点。
```

## 推荐小红书标题

- 我用 REDSkill 做了一只世界杯负鼠看球搭子
- 对世界杯动手了：一键安装会报赛程的 Codex 负鼠
- 熬夜看球还要上班？我做了只背手球评负鼠

## 目录结构

```text
worldcup-opossum-pet-skill/
  README.md
  examples/
    prompts.md
    images/
      current-state-map.png
  worldcup-opossum-pet/
    SKILL.md
    agents/
      openai.yaml
    assets/
      reference/
        yellow-complete-state.png
    config/
      pet.config.example.json
    references/
      config-guide.md
      hatch-pet-integration.md
      output-contract.md
```

真正安装的是 `worldcup-opossum-pet/` 子目录；根目录和 `examples/` 是给 GitHub / 小红书展示用的。展示图可以静态，宠物本体必须是动态图集。
