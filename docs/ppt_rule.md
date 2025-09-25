# 用于生成“PPT模拟风格”HTML页面的核心准则

---

### **1. 核心布局原则：放弃响应式，拥抱“舞台”**

*   **准则 #1：【使用绝对定位】** 必须将“百分比定位”理解为以 `position: absolute` 为核心，配合 `top`, `left`, `width`, `height` 的百分比值来布局。这是模拟PPT幻灯片内元素固定位置感的唯一可靠方法。**不能**使用常规的文档流、Flexbox或Grid布局，因为它们会导致响应式重排。

*   **准则 #2：【建立“舞台”容器】** 每个页面都应该有一个主容器（例如，`<div class="slide-container">`），其 `position` 为 `relative`，宽高为 `100%`。所有内部元素都相对于这个“舞台”进行绝对定位。

### **2. 空间管理原则：严禁滚动**

*   **准则 #3：【内容必须可见】** 幻灯片的核心原则是**所有内容必须在一屏内可见，严禁出现滚动条**。在布局时，所有元素的尺寸和间距（`height`, `margin`, `padding`）的百分比总和不能超过100%。

*   **准则 #4：【优先保证内容】** 如果内容过多，应优先考虑**减小字体、缩减内/外边距、或调整布局**，而不是让容器出现滚动条。

*   **准则 #5：【区分关键信息】** 在同一个组件内，不要为所有文本（如`<p>`）设置单一的全局字号。应为需要强调的、作为视觉焦点的关键信息（如小标题、日期、关键数据）定义一个专门的CSS类（例如 `.key-info`），并为其设置一个比正文更大的字号，以增强信息层级感。

*   **准则 #6：【动态字体大小】** 为确保文字大小能随屏幕等比例缩放，应使用视口单位（`vw` 或 `vh`）来定义字体大小，而不是固定的像素（`px`）。例如 `font-size: 1.5vw;`。

### **3. 复杂图表处理原则：预见问题，备好方案**

*   **准则 #5：【预判库的局限性】** 当使用 `mermaid.js` 这类图表库时，必须预判到它的默认渲染尺寸是基于内容的，**不会自动填满容器**。不能简单地把它放进一个 `div` 了事。

*   **准则 #6：【JS是最终手段】** 解决图表库的缩放问题，CSS往往是无力的。必须准备好使用JavaScript，在图表渲染完成后，通过**修改SVG的`preserveAspectRatio`属性**来控制缩放行为：
    *   要**等比缩放**（不变形），设置为 `xMidYMid meet`。
    *   要**拉伸填满**（会变形），设置为 `none`。

*   **准则 #7：【备用纯CSS方案】** 对于“总-分”或“脑图”结构，如果图表库无法满足精细的布局要求，应准备好备用方案，即“**CSS绝对定位节点 + CSS绝对定位连接线**”的纯净方案。虽然繁琐，但控制力最强。

### **4. 交付完整性原则：所见即所得**

*   **准则 #8：【禁止占位符】** 在任何迭代过程中，交付的代码文件都必须是**完整且可独立运行**的。绝不能使用“(内容不变)”之类的占位符，避免造成信息缺失和误解。

---

## 附录：布局模板代码样例

### A. 二分栏布局 (50/50 Split)

**HTML 结构:**
```html
<div class="two-column-container">
    <div class="column column-left">
        <!-- 左栏内容 -->
    </div>
    <div class="column column-right">
        <!-- 右栏内容 -->
    </div>
</div>
```

**CSS 核心:**
```css
.two-column-container {
    position: relative;
    width: 100%;
    height: 85%; /* 可根据内容调整 */
}

.column {
    position: absolute;
    top: 0;
    width: 48%; /* 留出 4% 间距 */
    height: 100%;
    box-sizing: border-box;
}

.column-left { left: 0; }
.column-right { left: 52%; }
```

### B. 三卡片横向布局

**HTML 结构:**
```html
<div class="three-step-container">
    <div id="step1" class="step-card">...</div>
    <div id="arrow1" class="step-arrow">→</div>
    <div id="step2" class="step-card">...</div>
    <div id="arrow2" class="step-arrow">→</div>
    <div id="step3" class="step-card">...</div>
</div>
```

**CSS 核心:**
```css
.three-step-container {
    position: relative;
    width: 100%;
    height: 75%;
}

.step-card {
    position: absolute;
    width: 30%;
    height: 100%;
    top: 0;
}

.step-arrow {
    position: absolute;
    top: 45%;
}

#step1 { left: 2%; }
#arrow1 { left: 32.5%; }
#step2 { left: 35%; }
#arrow2 { left: 65.5%; }
#step3 { left: 68%; }
```

### C. 四宫格布局 (2x2 Quadrant)

**HTML 结构:**
```html
<div class="quadrant-layout-container">
    <div class="quadrant-card q-top-left">...</div>
    <div class="quadrant-card q-top-right">...</div>
    <div class="quadrant-card q-bottom-left">...</div>
    <div class="quadrant-card q-bottom-right">...</div>
</div>
```

**CSS 核心:**
```css
.quadrant-layout-container {
    position: relative;
    width: 100%;
    height: 75%;
}

.quadrant-card {
    position: absolute;
    width: 48%;
    height: 48%;
    box-sizing: border-box;
}

.q-top-left    { top: 0;   left: 0; }
.q-top-right   { top: 0;   left: 52%; }
.q-bottom-left { top: 52%; left: 0; }
.q-bottom-right{ top: 52%; left: 52%; }
```

### D. 中心辐射脑图布局 (CSS-only)

**HTML 结构:**
```html
<div class="mindmap-container">
    <!-- Lines -->
    <div class="mindmap-line line-v"></div>
    <div class="mindmap-line line-h-left"></div>
    <div class="mindmap-line line-h-right"></div>
    <!-- Nodes -->
    <div class="mindmap-node mindmap-center">...</div>
    <div class="mindmap-node mindmap-top">...</div>
    <div class="mindmap-node mindmap-left">...</div>
    <div class="mindmap-node mindmap-right">...</div>
</div>
```

**CSS 核心:**
```css
.mindmap-container { position: relative; width: 100%; height: 80%; }
.mindmap-node { position: absolute; width: 24%; height: 30%; }
.mindmap-line { position: absolute; background-color: var(--accent-color); }

.mindmap-center { top: 50%; left: 50%; transform: translate(-50%, -50%); }
.mindmap-top { top: 0; left: 50%; transform: translateX(-50%); }
.mindmap-left { top: 50%; left: 0; transform: translateY(-50%); }
.mindmap-right { top: 50%; right: 0; transform: translateY(-50%); }

.line-v { left: 50%; top: 15%; width: 2px; height: 20%; transform: translateX(-50%); }
.line-h-left { left: 12%; top: 50%; width: 24%; height: 2px; transform: translateY(-50%); }
.line-h-right { right: 12%; top: 50%; width: 24%; height: 2px; transform: translateY(-50%); }
```
