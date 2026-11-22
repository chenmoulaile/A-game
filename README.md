# LAST STAND / 最后防线

雨夜末日军事题材的网页端 3D 第一人称射击战役。场景、人物、枪械、天气、粒子和音效均由程序化几何、材质与 Web Audio 合成生成，不使用外部模型、贴图或音频素材。

## 在线游玩

- 游戏入口：[`dist/index.html`](dist/index.html)
- 使用桌面端现代浏览器通过 HTTP 访问，不能只下载入口 HTML。
- 入口会读取 `dist/payload/00.b64` 至 `07.b64`，组合后解压完整游戏页面。
- 游戏使用 Three.js r180 官方 CDN 模块，首次运行需要访问 CDN。
- 将仓库根目录发布到 GitHub Pages 后，可访问 `/dist/`。

## 本地运行

完整源码构建器使用 Node.js 内置 API，不需要安装 npm 依赖：

```bash
cd work
npm test
npm run build
```

构建会生成单个 `dist/index.html` 游戏页面。运行时需访问 Three.js CDN。推荐从仓库根目录启动静态服务器：

```bash
python -m http.server 8000
```

打开 `http://localhost:8000/dist/`。浏览器对 `file://` 下的 ES 模块和 fetch 有安全限制，推荐使用 HTTP 服务器。

## 操作

| 操作 | 按键 |
|---|---|
| 移动 | W / A / S / D |
| 冲刺 | Shift |
| 射击 / 开镜 | 鼠标左键 / 右键 |
| 换弹 / 切换武器 | R / 1—5 |
| 互动 / 救援 | E |
| 暂停 / 释放鼠标 | Esc |

点击开始后会请求 Pointer Lock；按 Esc 释放鼠标。

## 战役内容

- 五幕流程：展览日、正门防线、防线突破、内场与看台、终幕命令。
- 五种枪械：M4A1、MP5K、M110、M870、P226；独立射速、伤害、弹匣、散布、后坐和换弹时间。
- 感染体：普通感染者、Runner、Brute、Spitter、Heavy；具有人形四肢、头部弱点、追击与攻击动作。
- 24 名友军单位自动交战，包含队长、医疗兵、破门手、机枪组和步枪兵。
- 两项可选支线：接通应急广播、救出被困坦克乘员；全部完成可解锁隐藏结局 H1。
- 七种正向结局：S1—S3、E1—E3 与 H1。机枪组始终留守；三辆坦克一辆留下、两辆撤离，乘员存活。
- 结局评分：平民救援 0.5、坦克完好度 0.2、坚守时长 0.18、机枪组留守时长 0.12。

## 工程结构

```text
.github/workflows/ci.yml   GitHub Actions 测试与构建
work/
  index.template.html      HTML / HUD 模板
  package.json             Node.js 项目脚本
  scripts/build.mjs        单文件构建器
  src/core.js              武器、结局、FSM、A*、空间哈希等逻辑
  src/main.js              Three.js 游戏主体
  test/core.test.js        核心逻辑单元测试
dist/index.html             游戏入口
dist/payload/*.b64          压缩游戏页面资源分段
tmp/.gitkeep                临时目录占位
```

## 测试与构建

```bash
cd work
npm test
npm run build
```

测试使用 Node.js 内置测试运行器，覆盖武器表、评分与七种结局、换弹逻辑、Blackboard / FSM、A* 禁止切角和空间哈希分离。CI 会在 push 与 pull request 时执行测试和构建。

## 技术说明与边界

- Three.js r180 通过官方 CDN 加载；场景与声音为程序化生成，不加载外部美术素材。
- 单玩家本地战役，无联网联机、账号或云存档。
- AI 已拆分 Blackboard 感知、带驻留时间的 FSM、A* 寻路工具和空间哈希分离模块；当前实时战斗单位使用直接追击与局部避让以优先保证节奏和帧率，A* 工具已独立测试，尚未接管所有实时单位。
- 这是首版可玩战役实现，不等同于高预算商业 FPS；复杂小队战术、真实动画骨骼资产与物理材质穿透仍有后续深化空间。
- 当前自动化容器无法访问外部 CDN，因此已完成静态构建和核心单元测试，但真实 WebGL 渲染仍需在可访问 CDN 的桌面浏览器复核。
