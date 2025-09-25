# 数字人科普-LLM PPT演示

一个基于HTML/CSS/JavaScript的PPT风格演示项目，展示LLM和AI相关技术概念。

## 项目特点

- 🎨 PPT风格的幻灯片设计
- 📱 响应式布局，支持多种屏幕尺寸
- 🎯 使用绝对定位实现精确布局
- 📊 集成Mermaid图表库
- 🖼️ 支持图片缩放和平移功能

## 在线访问

部署在Vercel: [项目链接](https://your-project-name.vercel.app)

## 本地运行

1. 克隆项目
```bash
git clone <repository-url>
cd 数字人科普-LLM
```

2. 启动本地服务器
```bash
# 使用Python
python -m http.server 8000

# 或使用Node.js
npx serve .
```

3. 访问 `http://localhost:8000/`

## 项目结构

```
├── index.html        # 主页面
├── page1-10.html     # 各个幻灯片页面
├── style.css         # 样式文件
├── pics/             # 图片资源
├── vercel.json       # Vercel部署配置
└── README.md         # 项目说明
```

## 部署到Vercel

1. 推送代码到GitHub
2. 在Vercel中导入GitHub仓库
3. Vercel会自动检测并部署项目

## 技术栈

- HTML5/CSS3
- JavaScript (ES6+)
- Mermaid.js (图表库)
- Font Awesome (图标库)