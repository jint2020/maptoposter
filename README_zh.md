# 城市地图海报生成器

生成精美、极简风格的世界各地城市地图海报。

<img src="posters/singapore_neon_cyberpunk_20260118_153328.png" width="250">
<img src="posters/dubai_midnight_blue_20260118_140807.png" width="250">

## 示例

| 国家      | 城市            | 主题            | 海报 |
|:------------:|:--------------:|:---------------:|:------:|
| USA          | San Francisco  | sunset          | <img src="posters/san_francisco_sunset_20260118_144726.png" width="250"> |
| 西班牙      | Barcelona      | warm_beige      | <img src="posters/barcelona_warm_beige_20260118_140048.png" width="250"> |
| 意大利      | Venice         | blueprint       | <img src="posters/venice_blueprint_20260118_140505.png" width="250"> |
| 日本        | Tokyo          | japanese_ink    | <img src="posters/tokyo_japanese_ink_20260118_142446.png" width="250"> |
| 印度        | Mumbai         | contrast_zones  | <img src="posters/mumbai_contrast_zones_20260118_145843.png" width="250"> |
| 摩洛哥      | Marrakech      | terracotta      | <img src="posters/marrakech_terracotta_20260118_143253.png" width="250"> |
| 新加坡      | Singapore      | neon_cyberpunk  | <img src="posters/singapore_neon_cyberpunk_20260118_153328.png" width="250"> |
| 澳大利亚    | Melbourne      | forest          | <img src="posters/melbourne_forest_20260118_153446.png" width="250"> |
| 阿联酋      | Dubai          | midnight_blue   | <img src="posters/dubai_midnight_blue_20260118_140807.png" width="250"> |
| USA          | Seattle        | emerald         | <img src="posters/seattle_emerald_20260124_162244.png" width="250"> |

## 安装

### 使用 uv（推荐）

请确保已安装 [uv](https://docs.astral.sh/uv/)。在脚本前添加 `uv run` 会自动创建和管理虚拟环境。

```bash
# 首次运行将自动安装依赖
uv run ./create_map_poster.py --city "Paris" --country "France"

# 或者先显式同步依赖（使用锁定版本）
uv sync --locked
uv run ./create_map_poster.py --city "Paris" --country "France"
```

### 使用 pip + venv

```bash
python -m venv .venv
source .venv/bin/activate  # Windows 上: .venv\Scripts\activate
pip install -r requirements.txt
```

## 使用方法

### 生成海报

如果使用 `uv`:

```bash
uv run ./create_map_poster.py --city <城市> --country <国家> [选项]
```

否则（pip + venv）:

```bash
python create_map_poster.py --city <城市> --country <国家> [选项]
```

### 必需选项

| 选项 | 简写 | 说明 |
|--------|-------|-------------|
| `--city` | `-c` | 城市名称（用于地理编码） |
| `--country` | `-C` | 国家名称（用于地理编码） |

### 可选参数

| 选项 | 简写 | 说明 | 默认值 |
|--------|-------|-------------|---------|
| **可选:** `--latitude` | `-lat` | 覆盖纬度中心点（与 --longitude 配合使用） | |
| **可选:** `--longitude` | `-long` | 覆盖经度中心点（与 --latitude 配合使用） | |
| **可选:** `--country-label` | | 覆盖海报上显示的国家文本 | |
| **可选:** `--theme` | `-t` | 主题名称 | terracotta |
| **可选:** `--distance` | `-d` | 地图半径（米） | 18000 |
| **可选:** `--list-themes` | | 列出所有可用主题 | |
| **可选:** `--all-themes` | | 为所有可用主题生成海报 | |
| **可选:** `--width` | `-W` | 图像宽度（英寸） | 12（最大: 20） |
| **可选:** `--height` | `-H` | 图像高度（英寸） | 16（最大: 20） |

### 多语言支持 - i18n

使用 Google Fonts 自定义字体，以您的语言显示城市和国家名称：

| 选项 | 简写 | 说明 |
|--------|-------|-------------|
| `--display-city` | `-dc` | 城市的自定义显示名称（例如 "东京"） |
| `--display-country` | `-dC` | 国家的自定义显示名称（例如 "日本"） |
| `--font-family` | | Google Fonts 字体家族名称（例如 "Noto Sans JP"） |
| `--custom-font` | | 本地字体文件路径，或 `fonts/custom` 目录中的字体文件名（例如 `NotoSansSC-Regular.ttf`） |

**示例：**

```bash
# 日语
python create_map_poster.py -c "Tokyo" -C "Japan" -dc "東京" -dC "日本" --font-family "Noto Sans JP"

# 韩语
python create_map_poster.py -c "Seoul" -C "South Korea" -dc "서울" -dC "대한민국" --font-family "Noto Sans KR"

# 阿拉伯语
python create_map_poster.py -c "Dubai" -C "UAE" -dc "دبي" -dC "الإمارات" --font-family "Cairo"
```

**注意**：字体会自动从 Google Fonts 下载并缓存在 `fonts/cache/` 目录中。

### 本地自定义字体（`--custom-font`）

如果你想使用自己下载的字体文件（不通过 Google Fonts）：

1. 将字体文件放到 `fonts/custom/` 目录（例如：`fonts/custom/MyFont-Regular.ttf`）
2. 使用 `--custom-font` 参数运行

```bash
# 使用 fonts/custom/ 中的字体文件名
python create_map_poster.py -c "Beijing" -C "China" -dc "北京" -dC "中国" --custom-font "MyFont-Regular.ttf"

# 或使用绝对/相对路径
python create_map_poster.py -c "Tokyo" -C "Japan" -dc "東京" -dC "日本" --custom-font "/path/to/your/font.ttf"
```

**优先级**：`--custom-font` > `--font-family` > 默认本地 Roboto 字体。

### 分辨率指南（300 DPI）

使用 `-W` 和 `-H` 参数指定目标分辨率：

| 目标 | 分辨率 (px) | 英寸 (-W / -H) |
|--------|-----------------|------------------|
| **Instagram 帖子** | 1080 x 1080 | 3.6 x 3.6 |
| **手机壁纸** | 1080 x 1920 | 3.6 x 6.4 |
| **HD 壁纸** | 1920 x 1080 | 6.4 x 3.6 |
| **4K 壁纸** | 3840 x 2160 | 12.8 x 7.2 |
| **A4 打印** | 2480 x 3508 | 8.3 x 11.7 |

### 使用示例

#### 基础示例

```bash
# 使用默认主题的简单用法
python create_map_poster.py -c "Paris" -C "France"

# 自定义主题和距离
python create_map_poster.py -c "New York" -C "USA" -t noir -d 12000
```

#### 多语言示例（非拉丁字母）

以原生文字显示城市名称：

```bash
# 日语
python create_map_poster.py -c "Tokyo" -C "Japan" -dc "東京" -dC "日本" --font-family "Noto Sans JP" -t japanese_ink

# 韩语
python create_map_poster.py -c "Seoul" -C "South Korea" -dc "서울" -dC "대한민국" --font-family "Noto Sans KR" -t midnight_blue

# 泰语
python create_map_poster.py -c "Bangkok" -C "Thailand" -dc "กรุงเทพมหานคร" -dC "ประเทศไทย" --font-family "Noto Sans Thai" -t sunset

# 阿拉伯语
python create_map_poster.py -c "Dubai" -C "UAE" -dc "دبي" -dC "الإمارات" --font-family "Cairo" -t terracotta

# 简体中文
python create_map_poster.py -c "Beijing" -C "China" -dc "北京" -dC "中国" --font-family "Noto Sans SC"

# 高棉语
python create_map_poster.py -c "Phnom Penh" -C "Cambodia" -dc "ភ្នំពេញ" -dC "កម្ពុជា" --font-family "Noto Sans Khmer"
```

#### 高级示例

```bash
# 标志性的网格模式
python create_map_poster.py -c "New York" -C "USA" -t noir -d 12000           # 曼哈顿网格
python create_map_poster.py -c "Barcelona" -C "Spain" -t warm_beige -d 8000   # 扩展区

# 滨水区与运河
python create_map_poster.py -c "Venice" -C "Italy" -t blueprint -d 4000       # 运河网络
python create_map_poster.py -c "Amsterdam" -C "Netherlands" -t ocean -d 6000  # 同心运河
python create_map_poster.py -c "Dubai" -C "UAE" -t midnight_blue -d 15000     # 棕榈岛与海岸线

# 放射状模式
python create_map_poster.py -c "Paris" -C "France" -t pastel_dream -d 10000   # 奥斯曼大道
python create_map_poster.py -c "Moscow" -C "Russia" -t noir -d 12000          # 环形公路

# 有机老城
python create_map_poster.py -c "Tokyo" -C "Japan" -t japanese_ink -d 15000    # 密集有机街道
python create_map_poster.py -c "Marrakech" -C "Morocco" -t terracotta -d 5000 # 麦地那迷宫
python create_map_poster.py -c "Rome" -C "Italy" -t warm_beige -d 8000        # 古代布局

# 沿海城市
python create_map_poster.py -c "San Francisco" -C "USA" -t sunset -d 10000    # 半岛网格
python create_map_poster.py -c "Sydney" -C "Australia" -t ocean -d 12000      # 港口城市
python create_map_poster.py -c "Mumbai" -C "India" -t contrast_zones -d 18000 # 沿海半岛

# 河流城市
python create_map_poster.py -c "London" -C "UK" -t noir -d 15000              # 泰晤士河曲线
python create_map_poster.py -c "Budapest" -C "Hungary" -t copper_patina -d 8000  # 多瑙河分叉

# 覆盖中心坐标
python create_map_poster.py --city "New York" --country "USA" -lat 40.776676 -long -73.971321 -t noir

# 列出可用主题
python create_map_poster.py --list-themes

# 为每个主题生成海报
python create_map_poster.py -c "Tokyo" -C "Japan" --all-themes
```

### 距离指南

| 距离 | 适用场景 |
|----------|----------|
| 4000-6000米 | 小型/密集城市（威尼斯、阿姆斯特丹市中心） |
| 8000-12000米 | 中型城市，聚焦市中心（巴黎、巴塞罗那） |
| 15000-20000米 | 大都市，全城视角（东京、孟买） |

## 主题

`themes/` 目录下有 17 个可用主题：

| 主题 | 风格 |
|-------|-------|
| `gradient_roads` | 平滑渐变着色 |
| `contrast_zones` | 高对比城市密度 |
| `noir` | 纯黑背景，白色道路 |
| `midnight_blue` | 深蓝色背景，金色道路 |
| `blueprint` | 建筑蓝图美学 |
| `neon_cyberpunk` | 暗色配霓虹粉/青色 |
| `warm_beige` | 复古棕褐色 |
| `pastel_dream` | 柔和淡雅粉彩 |
| `japanese_ink` | 极简水墨风格 |
| `emerald`      | 浓郁深绿美学 |
| `forest` | 深绿与鼠尾草色 |
| `ocean` | 蓝色与蓝绿色，适合沿海城市 |
| `terracotta` | 地中海温暖色调 |
| `sunset` | 暖橙与粉红 |
| `autumn` | 季节性褐橙与红色 |
| `copper_patina` | 氧化铜美学 |
| `monochrome_blue` | 单一蓝色系 |

## 输出

海报保存到 `posters/` 目录，格式如下：

```text
{城市}_{主题}_{YYYYMMDD_HHMMSS}.png
```

## 添加自定义主题

在 `themes/` 目录创建 JSON 文件：

```json
{
  "name": "My Theme",
  "description": "Description of the theme",
  "bg": "#FFFFFF",
  "text": "#000000",
  "gradient_color": "#FFFFFF",
  "water": "#C0C0C0",
  "parks": "#F0F0F0",
  "road_motorway": "#0A0A0A",
  "road_primary": "#1A1A1A",
  "road_secondary": "#2A2A2A",
  "road_tertiary": "#3A3A3A",
  "road_residential": "#4A4A4A",
  "road_default": "#3A3A3A"
}
```

## 项目结构

```text
map_poster/
├── create_map_poster.py    # 主脚本
├── font_management.py      # 字体加载与 Google Fonts 集成
├── themes/                 # 主题 JSON 文件
├── fonts/                  # 字体文件
│   ├── Roboto-*.ttf        # 默认 Roboto 字体
│   └── cache/              # 下载的 Google Fonts（自动生成）
├── posters/                # 生成的海报
└── README.md
```

## 开发者指南

想要扩展或修改脚本的贡献者快速参考。

### 贡献者指南

- 欢迎提交错误修复
- 不要提交用户界面（网页/桌面）
- 暂时不要 Docker 化
- 如果你凭感觉写代码，请测试并查看修复前后的海报版本
- 在开发大型功能前，请先在讨论区/问题区询问是否会合并

### 架构概述

```text
┌─────────────────┐     ┌──────────────┐     ┌─────────────────┐
│   CLI 解析器    │────▶│   地理编码    │────▶│   数据获取      │
│   (argparse)    │     │  (Nominatim) │     │    (OSMnx)      │
└─────────────────┘     └──────────────┘     └─────────────────┘
                                                     │
                        ┌──────────────┐             ▼
                        │    输出      │◀────┌─────────────────┐
                        │  (matplotlib)│     │   渲染          │
                        └──────────────┘     │  (matplotlib)   │
                                             └─────────────────┘
```

### 关键函数

| 函数 | 用途 | 修改时机 |
|----------|---------|----------------|
| `get_coordinates()` | 城市 → 经纬度（Nominatim） | 切换地理编码提供商 |
| `create_poster()` | 主渲染流水线 | 添加新地图图层 |
| `get_edge_colors_by_type()` | 按 OSM highway 标签的道路颜色 | 更改道路样式 |
| `get_edge_widths_by_type()` | 按重要性的道路宽度 | 调整线宽 |
| `create_gradient_fade()` | 顶部/底部渐变效果 | 修改渐变叠加 |
| `load_theme()` | JSON 主题 → 字典 | 添加新主题属性 |
| `is_latin_script()` | 检测文字脚本以进行排版 | 支持新文字脚本 |
| `load_fonts()` | 加载自定义/默认字体 | 更改字体加载逻辑 |

### 渲染图层（z-order）

```text
z=11  文本标签（城市、国家、坐标）
z=10  渐变遮罩（顶部和底部）
z=3   道路（通过 ox.plot_graph）
z=2   公园（绿色多边形）
z=1   水体（蓝色多边形）
z=0   背景颜色
```

### OSM 道路类型 → 道路层级

```python
# 在 get_edge_colors_by_type() 和 get_edge_widths_by_type() 中
motorway, motorway_link     → 最粗 (1.2)，最深色
trunk, primary              → 较粗 (1.0)
secondary                   → 中等 (0.8)
tertiary                    → 较细 (0.6)
residential, living_street  → 最细 (0.4)，最浅色
```

### 排版与文字检测

脚本自动检测文字脚本以应用适当的排版：

- **拉丁字母**（英语、法语、西班牙语等）：应用字母间距，产生优雅的 "P  A  R  I  S" 效果
- **非拉丁字母**（日语、阿拉伯语、泰语、韩语等）：自然间距 "东京"（字符之间无间隙）

文字检测使用 Unicode 范围（拉丁字母为 U+0000-U+024F）。如果超过 80% 的字母字符是拉丁字母，则应用间距。

### 添加新功能

**新地图图层（例如铁路）：**

```python
# 在 create_poster() 中，获取公园后：
try:
    railways = ox.features_from_point(point, tags={'railway': 'rail'}, dist=dist)
except:
    railways = None

# 然后在道路之前绘制：
if railways is not None and not railways.empty:
    railways = railways.to_crs(g_proj.graph["crs"])
    railways.plot(ax=ax, color=THEME['railway'], linewidth=0.5, zorder=2.5)
```

**新主题属性：**

1. 添加到主题 JSON：`"railway": "#FF0000"`
2. 在代码中使用：`THEME['railway']`
3. 在 `load_theme()` 默认字典中添加回退

### 排版定位

所有文本使用 `transform=ax.transAxes`（0-1 归一化坐标）：

```text
y=0.14  城市名称（拉丁字母带间距）
y=0.125 装饰线
y=0.10  国家名称
y=0.07  坐标
y=0.02  归属（右下角）
```

### 常用 OSMnx 模式

```python
# 获取所有建筑
buildings = ox.features_from_point(point, tags={'building': True}, dist=dist)

# 获取特定设施
cafes = ox.features_from_point(point, tags={'amenity': 'cafe'}, dist=dist)

# 不同的网络类型
G = ox.graph_from_point(point, dist=dist, network_type='drive')  # 仅道路
G = ox.graph_from_point(point, dist=dist, network_type='bike')   # 自行车道
G = ox.graph_from_point(point, dist=dist, network_type='walk')   # 行人
```

### 性能提示

- 大的 `dist` 值（>20公里）= 下载慢 + 内存占用大
- 本地缓存坐标以避免 Nominatim 速率限制
- 使用 `network_type='drive'` 而非 `'all'` 以加快渲染
- 将 `dpi` 从 300 降低到 150 以快速预览