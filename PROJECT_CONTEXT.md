# QA/Testing Learning Platform - Project Context

## 📋 Tổng quan dự án

Hệ thống học tập QA/Testing với các bài học tương tác, được xây dựng dưới dạng **self-contained HTML files** - mỗi file là một bài học hoàn chỉnh với đầy đủ CSS, JavaScript và nội dung.

**Đặc điểm chính:**
- 🎯 Mỗi file HTML là một bài học độc lập, không phụ thuộc external files
- 🎨 Design system thống nhất, màu sắc thay đổi theo từng bài
- 📱 Responsive design, tối ưu cho mobile và desktop
- 💾 Progress tracking qua localStorage
- 📊 Quiz system với auto-grading và postMessage API
- 🔗 Navigation giữa các bài qua index.html
- Ngôn ngữ của bài học là tiếng Trung Quốc.

---

## 🎨 Design System

### Color Variables (CSS Custom Properties)

Mỗi bài học có bộ màu riêng, định nghĩa trong `:root`:

```css
:root{
  --day:#059669;           /* Màu chính của bài học (thay đổi mỗi ngày) */
  --day-d:#047857;         /* Màu đậm hơn (hover states) */
  --day-bg:rgba(5,150,105,.08); /* Background nhạt */
  
  /* Fixed colors - giống nhau mọi bài */
  --bg:#f8fafc;           /* Background trang */
  --card:#fff;            /* Background card/section */
  --ink:#0f172a;          /* Text color chính */
  --ink2:#334155;         /* Text color phụ */
  --muted:#64748b;        /* Text muted/placeholder */
  --border:#e2e8f0;       /* Border color */
  --code-bg:#1e2030;      /* Code block background */
  --code-ink:#a6accd;     /* Code block text */
  
  /* Semantic colors */
  --green:#16a34a;        /* Success/correct */
  --red:#dc2626;          /* Error/wrong */
  --amber:#d97706;        /* Warning/explanation */
  --blue:#3b82f6;         /* Info */
}
```

**Ví dụ màu theo bài:**
- Day 04: `--day:#0d9488` (Teal)
- Day 05: `--day:#059669` (Emerald)

---

## 🏗️ Component Structure

### 1. Header (.hd)

```css
.hd{
  background:linear-gradient(135deg,var(--day) 0%,var(--day-d) 100%);
  color:#fff;
  padding:28px 28px 24px;
  border-radius:14px;
  margin-bottom:20px;
  box-shadow:0 8px 24px rgba(...)
}
```

**Cấu trúc HTML:**
```html
<div class="hd">
  <div class="hd-tag">STAGE 2 · LINUX DAY 05</div>
  <h1>连接查询 / 自关联 / 子查询</h1>
  <p>Mô tả ngắn gọn về nội dung bài học...</p>
  <div class="hd-stats">
    <div><b>3</b>种JOIN</div>
    <div><b>10</b>实战练习</div>
    <div><b>12</b>面试题</div>
    <div><b>15</b>测验题</div>
  </div>
</div>
```

**Elements:**
- `.hd-tag`: Label nhỏ (STAGE X · LINUX DAY XX)
- `.hd-stats`: Statistics với `<b>` cho số và Space Mono font

---

### 2. Navigation Dots (#dots)

```css
#dots{display:flex;gap:8px;justify-content:center;margin:0 0 16px;flex-wrap:wrap}
.dot{width:10px;height:10px;border-radius:50%;background:#cbd5e1;cursor:pointer;transition:all .2s}
.dot.a{background:var(--day);transform:scale(1.4)}  /* Active dot */
.dot.d{background:var(--green)}  /* Done/completed dot */
```

**JavaScript tạo dots tự động:**
```javascript
const dots=document.getElementById('dots');
for(let i=0;i<pans.length;i++){
  const d=document.createElement('span');
  d.className='dot'+(i===0?' a':'');
  d.onclick=()=>go(i);
  dots.appendChild(d);
}
```

---

### 3. Tabs Navigation (.dn > .ts > .t)

```css
.dn{
  background:var(--card);
  border:1px solid var(--border);
  border-radius:12px;
  padding:6px;
  display:flex;gap:4px;
  margin-bottom:18px;
  overflow-x:auto
}
.t{
  flex:1;
  padding:10px 12px;
  border:none;
  background:transparent;
  color:var(--muted);
  font-size:13px;
  font-weight:600;
  cursor:pointer;
  border-radius:8px;
  white-space:nowrap;
  font-family:inherit;
  transition:all .2s
}
.t.a{
  background:var(--day);
  color:#fff;
  box-shadow:0 2px 8px rgba(...)
}
```

**Cấu trúc HTML:**
```html
<div class="dn">
  <div class="ts">
    <button class="t a" data-p="0">📖 知识讲解</button>
    <button class="t" data-p="1">🖼️ 图解演示</button>
    <button class="t" data-p="2">🛠️ 实战练习</button>
    <button class="t" data-p="3">💬 面试精讲</button>
    <button class="t" data-p="4">✅ 测验</button>
  </div>
</div>
```

**5 tabs cố định cho mọi bài:**
1. 📖 知识讲解 - Kiến thức lý thuyết
2. 🖼️ 图解演示 - Visualizations/demos
3. 🛠️ 实战练习 - Practice exercises
4. 💬 面试精讲 - Interview Q&A
5. ✅ 测验 - Quiz

---

### 4. Panels (.pan)

```css
.pan{display:none;animation:fade .3s}
.pan.a{display:block}
@keyframes fade{
  from{opacity:0;transform:translateY(6px)}
  to{opacity:1;transform:none}
}
```

**Mỗi panel tương ứng 1 tab, chỉ có 1 panel active (.a) tại 1 thời điểm.**

---

### 5. Section Cards (.sec)

```css
.sec{
  background:var(--card);
  border:1px solid var(--border);
  border-radius:14px;
  padding:22px;
  margin-bottom:18px
}
.sec h2{
  font-size:18px;
  font-weight:700;
  color:var(--ink);
  margin-bottom:14px;
  padding-bottom:10px;
  border-bottom:2px solid var(--day);
  display:flex;
  align-items:center;
  gap:8px
}
.sec h2 .ico{
  display:inline-flex;
  align-items:center;
  justify-content:center;
  width:32px;
  height:32px;
  background:var(--day-bg);
  color:var(--day-d);
  border-radius:8px;
  font-size:16px;
  font-weight:700;
  font-family:'Space Mono',monospace
}
```

**Cấu trúc HTML:**
```html
<div class="sec">
  <h2><span class="ico">1</span>连接查询概述</h2>
  <p>Nội dung section...</p>
  
  <h3>Subheading</h3>
  <p>More content...</p>
</div>
```

**H3 với vertical bar:**
```css
.sec h3{
  font-size:15px;
  font-weight:700;
  color:var(--ink);
  margin:18px 0 10px;
  display:flex;
  align-items:center;
  gap:6px
}
.sec h3::before{
  content:'';
  width:4px;
  height:16px;
  background:var(--day);
  border-radius:2px;
  display:inline-block
}
```

---

### 6. Code Blocks (.term)

```css
.term{
  background:var(--code-bg);
  color:var(--code-ink);
  font-family:'Space Mono',monospace;
  font-size:13px;
  padding:14px 16px;
  border-radius:8px;
  margin:10px 0;
  overflow-x:auto;
  line-height:1.7
}
```

**Syntax highlighting classes:**
```css
.term .p{color:#82aaff}  /* Purple - functions */
.term .c{color:#c3e88d}  /* Green - strings in SQL */
.term .o{color:#f78c6c}  /* Orange - operators */
.term .s{color:#ffcb6b}  /* Yellow - strings */
.term .k{color:#c792ea}  /* Purple - keywords */
.term .g{color:#676e95;font-style:italic}  /* Gray - comments */
.term .r{color:#f07178}  /* Red */
.term .w{color:#fff;font-weight:600}  /* White - emphasis */
.term .n{color:#f78c6c}  /* Numbers */
```

**Ví dụ sử dụng:**
```html
<div class="term">
<span class="k">SELECT</span> * <span class="k">FROM</span> students
<span class="k">WHERE</span> age <span class="o">></span> <span class="n">18</span>;

<span class="g">-- This is a comment</span>
<span class="k">INSERT INTO</span> scores <span class="k">VALUES</span>(<span class="s">'A001'</span>, <span class="n">95</span>);
</div>
```

**Inline code:**
```css
code.inl{
  font-family:'Space Mono',monospace;
  background:#f1f5f9;
  color:var(--day-d);
  padding:1px 6px;
  border-radius:4px;
  font-size:12.5px;
  font-weight:600
}
```

---

### 7. Info Boxes (.box)

```css
.box{
  padding:12px 14px;
  border-radius:8px;
  margin:10px 0;
  font-size:13px;
  line-height:1.7;
  border-left:4px solid
}
.box.info{background:#eff6ff;border-color:#3b82f6;color:#1e40af}
.box.tip{background:#f0fdf4;border-color:#16a34a;color:#15803d}
.box.danger{background:#fef2f2;border-color:#dc2626;color:#991b1b}
.box.warn{background:#fffbeb;border-color:#d97706;color:#92400e}
.box.ok{background:#ecfdf5;border-color:#10b981;color:#065f46}
```

**Usage:**
```html
<div class="box tip">
  <b>💡 记忆技巧</b><br>
  • 内连接 = 取交集<br>
  • 左连接 = 左表为主
</div>
```

---

### 8. Tables (.tw > table)

```css
.tw{
  overflow-x:auto;
  margin:12px 0;
  border-radius:8px;
  border:1px solid var(--border)
}
table{
  width:100%;
  border-collapse:collapse;
  font-size:13px;
  background:#fff;
  min-width:480px
}
th{
  background:#f1f5f9;
  font-weight:700;
  color:var(--ink);
  font-size:12.5px;
  padding:9px 12px;
  text-align:left
}
td{
  padding:9px 12px;
  text-align:left;
  border-bottom:1px solid var(--border)
}
tr:last-child td{border-bottom:none}
tr:hover td{background:#fafbfc}
```

**Cấu trúc HTML:**
```html
<div class="tw">
  <table>
    <tr>
      <th>连接类型</th>
      <th>说明</th>
      <th>关键字</th>
    </tr>
    <tr>
      <td>内连接</td>
      <td>查询两个表的交集</td>
      <td><code>INNER JOIN</code></td>
    </tr>
  </table>
</div>
```

---

### 9. Grid Layouts (.g2, .g3)

```css
.g2{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(260px,1fr));
  gap:12px;
  margin:12px 0
}
.g3{
  display:grid;
  grid-template-columns:repeat(auto-fit,minmax(200px,1fr));
  gap:12px;
  margin:12px 0
}
```

**Card component trong grid:**
```css
.card{
  background:#fafbfc;
  border:1px solid var(--border);
  border-radius:10px;
  padding:14px;
  transition:all .2s
}
.card:hover{
  transform:translateY(-2px);
  box-shadow:0 4px 12px rgba(0,0,0,.06);
  border-color:var(--day)
}
.card h4{
  font-size:14px;
  font-weight:700;
  color:var(--ink);
  margin-bottom:6px;
  display:flex;
  align-items:center;
  gap:6px
}
.card .num{
  display:inline-block;
  width:22px;
  height:22px;
  line-height:22px;
  text-align:center;
  background:var(--day);
  color:#fff;
  border-radius:50%;
  font-family:'Space Mono',monospace;
  font-size:11px;
  font-weight:700
}
```

---

### 10. Interview Q&A (.iq)

```css
.iq{
  background:var(--card);
  border:1px solid var(--border);
  border-radius:10px;
  margin-bottom:10px;
  overflow:hidden;
  transition:all .2s
}
.iq:hover{border-color:var(--day)}
.iq-hdr{
  padding:14px 16px;
  cursor:pointer;
  display:flex;
  align-items:center;
  gap:10px;
  user-select:none
}
.iq-hdr:hover{background:var(--day-bg)}
.iq-num{
  font-family:'Space Mono',monospace;
  font-weight:700;
  color:var(--day-d);
  font-size:13px;
  min-width:34px
}
.iq-q{
  flex:1;
  font-size:13.5px;
  font-weight:600;
  color:var(--ink);
  line-height:1.5
}
.iq-lv{
  font-size:10px;
  padding:2px 7px;
  border-radius:10px;
  font-weight:700;
  white-space:nowrap
}
.lv-b{background:#dcfce7;color:#15803d}  /* 基础 */
.lv-m{background:#fef3c7;color:#92400e}  /* 进阶 */
.lv-h{background:#fee2e2;color:#991b1b}  /* 陷阱/场景 */
.iq-tg{
  font-family:'Space Mono',monospace;
  color:var(--muted);
  transition:transform .2s
}
.iq.o .iq-tg{transform:rotate(45deg)}  /* Toggle icon when open */
.iq-body{
  display:none;
  padding:0 16px 16px;
  border-top:1px dashed var(--border);
  font-size:13px;
  line-height:1.75;
  color:var(--ink2)
}
.iq.o .iq-body{display:block}
```

**Cấu trúc HTML:**
```html
<div class="iq">
  <div class="iq-hdr" onclick="tq(this)">
    <div class="iq-num">Q1</div>
    <div class="iq-q">什么是内连接？</div>
    <div class="iq-lv lv-b">基础</div>
    <div class="iq-tg">+</div>
  </div>
  <div class="iq-body">
    <p>Answer content...</p>
  </div>
</div>
```

**JavaScript toggle:**
```javascript
function tq(hdr){
  const iq=hdr.parentElement;
  iq.classList.toggle('o');
}
```

---

### 11. Practice Exercises (.pt)

```css
.pt{
  background:#fff;
  border:1px solid var(--border);
  border-radius:10px;
  padding:14px 16px;
  margin-bottom:10px;
  display:flex;
  gap:12px;
  align-items:flex-start;
  transition:all .2s
}
.pt:hover{
  border-color:var(--day);
  box-shadow:0 2px 8px rgba(0,0,0,.04)
}
.pt.done{
  background:#f0fdf4;
  border-color:var(--green)
}
.ci{
  margin-top:3px;
  width:18px;
  height:18px;
  cursor:pointer;
  accent-color:var(--day);
  flex-shrink:0
}
.pt-body{flex:1}
.pt-t{
  font-size:13.5px;
  font-weight:600;
  color:var(--ink);
  margin-bottom:4px;
  line-height:1.5
}
.pt.done .pt-t{
  text-decoration:line-through;
  color:var(--muted)
}
.pt-rev{
  font-family:'Space Mono',monospace;
  font-size:12px;
  background:#f8fafc;
  color:var(--muted);
  padding:6px 10px;
  border-radius:6px;
  cursor:pointer;
  display:inline-block;
  border:1px dashed var(--border)
}
.pt-rev:hover{
  color:var(--day-d);
  border-color:var(--day)
}
.pt-ans{
  display:none;
  margin-top:8px;
  padding:10px 12px;
  background:var(--code-bg);
  color:var(--code-ink);
  font-family:'Space Mono',monospace;
  font-size:12.5px;
  border-radius:6px;
  line-height:1.8;
  overflow-x:auto
}
.pt-ans.s{display:block}
```

**Cấu trúc HTML:**
```html
<div class="pt">
  <input type="checkbox" class="ci" id="p1">
  <div class="pt-body">
    <div class="pt-t">1. Query all students...</div>
    <span class="pt-rev" onclick="tr(this)">查看答案 ›</span>
    <div class="pt-ans">
<span class="k">SELECT</span> * <span class="k">FROM</span> students;
    </div>
  </div>
</div>
```

**JavaScript:**
```javascript
function tr(el){
  const ans=el.nextElementSibling;
  ans.classList.toggle('s');
  el.textContent=ans.classList.contains('s')?'隐藏答案 ›':'查看答案 ›';
}

// Checkbox tracking
const cbs=document.querySelectorAll('.ci');
function upc(){
  const done=document.querySelectorAll('.ci:checked').length;
  document.getElementById('pc').textContent=done;
  cbs.forEach(cb=>{
    cb.closest('.pt').classList.toggle('done',cb.checked);
  });
}
cbs.forEach(cb=>cb.onchange=upc);
upc();
```

---

### 12. Quiz System (.qz)

```css
.qz{
  background:var(--card);
  border:1px solid var(--border);
  border-radius:10px;
  padding:18px;
  margin-bottom:14px
}
.qz-q{
  font-size:14px;
  font-weight:600;
  color:var(--ink);
  margin-bottom:12px;
  line-height:1.6
}
.qz-q .qn{
  display:inline-block;
  width:26px;
  height:26px;
  line-height:26px;
  text-align:center;
  background:var(--day);
  color:#fff;
  border-radius:50%;
  font-family:'Space Mono',monospace;
  font-size:12px;
  font-weight:700;
  margin-right:8px
}
.opt{
  display:block;
  padding:10px 14px;
  border:1.5px solid var(--border);
  border-radius:8px;
  margin-bottom:8px;
  cursor:pointer;
  font-size:13.5px;
  color:var(--ink2);
  transition:all .15s;
  line-height:1.5
}
.opt:hover{
  border-color:var(--day);
  background:var(--day-bg)
}
.opt.sel{
  border-color:var(--day);
  background:var(--day-bg);
  color:var(--day-d);
  font-weight:600
}
.opt.ok{
  border-color:var(--green);
  background:#f0fdf4;
  color:#15803d;
  font-weight:600
}
.opt.no{
  border-color:var(--red);
  background:#fef2f2;
  color:#991b1b;
  font-weight:600
}
.qz-ex{
  display:none;
  margin-top:10px;
  padding:10px 12px;
  background:#fffbeb;
  border-left:3px solid var(--amber);
  border-radius:6px;
  font-size:12.5px;
  color:#78350f;
  line-height:1.6
}
.qz-ex.s{display:block}
.qz-ex b{color:var(--amber)}
```

---

### 13. Page Navigation (.pnav)

**Luôn có ở cuối mỗi panel:**

```css
.pnav{
  display:flex;
  justify-content:space-between;
  gap:10px;
  margin-top:18px;
  flex-wrap:wrap
}
.pnav button{
  flex:1;
  min-width:120px;
  background:#fff;
  border:1.5px solid var(--border);
  color:var(--ink2);
  padding:11px 16px;
  border-radius:10px;
  cursor:pointer;
  font-family:inherit;
  font-size:13px;
  font-weight:600;
  transition:all .2s
}
.pnav button:hover{
  border-color:var(--day);
  color:var(--day-d)
}
.pnav button.pri{
  background:var(--day);
  color:#fff;
  border-color:var(--day)
}
.pnav button.pri:hover{background:var(--day-d)}
.pnav button:disabled{
  opacity:.4;
  cursor:not-allowed
}
```

**Pattern:**

- **Panel 0 (đầu):** `disabled` ← | `primary` → 图解演示
- **Panel 1-3:** `← 上一节名` | `primary` → 下一节名
- **Panel 4 (cuối):** `← 面试精讲` | `primary disabled` 已是最后一节 ✓

```html
<!-- Panel 0 -->
<div class="pnav">
  <button disabled>← 上一节</button>
  <button class="pri" onclick="go(1)">下一节 图解演示 →</button>
</div>

<!-- Panel 1 -->
<div class="pnav">
  <button onclick="go(0)">← 知识讲解</button>
  <button class="pri" onclick="go(2)">下一节 实战练习 →</button>
</div>

<!-- Panel 4 (cuối) -->
<div class="pnav">
  <button onclick="go(3)">← 面试精讲</button>
  <button class="pri" disabled>已是最后一节 ✓</button>
</div>
```

---

## 📱 Mobile Responsive

**Breakpoint: `@media(max-width:640px)`**

```css
@media(max-width:640px){
  .wrap{padding:14px 12px 40px}
  .hd{padding:20px 18px 16px}
  .hd h1{font-size:19px}
  .hd p{font-size:12px}
  .sec{padding:16px 14px}
  .sec h2{font-size:16px}
  .sec h3{font-size:14px}
  .t{font-size:11px;padding:8px 6px}
  .dn{padding:4px}
  .term{font-size:11.5px;padding:10px 12px}
  .qbtn{padding:11px 18px;font-size:13px}
  .pnav button{font-size:12px;padding:9px 12px}
  .iq-hdr{padding:12px 12px}
  .iq-q{font-size:12.5px}
  table{font-size:12px}
  th,td{padding:7px 9px}
  .g2,.g3{grid-template-columns:1fr}
  .venn-grid{grid-template-columns:1fr}
}
```

---

## 🎯 JavaScript Core Functions

### Navigation System

```javascript
const pans=document.querySelectorAll('.pan');
const tabs=document.querySelectorAll('.t');
const dots=document.getElementById('dots');

// Create dots
for(let i=0;i<pans.length;i++){
  const d=document.createElement('span');
  d.className='dot'+(i===0?' a':'');
  d.onclick=()=>go(i);
  dots.appendChild(d);
}

// Panel switching
function go(i){
  tabs.forEach((t,k)=>t.classList.toggle('a',k===i));
  pans.forEach((p,k)=>p.classList.toggle('a',k===i));
  document.querySelectorAll('.dot').forEach((d,k)=>d.classList.toggle('a',k===i));
  window.scrollTo({top:0,behavior:'smooth'});
}

tabs.forEach((t,i)=>{
  t.onclick=()=>go(i);
});
```

### Practice Toggle

```javascript
function tr(el){
  const ans=el.nextElementSibling;
  ans.classList.toggle('s');
  el.textContent=ans.classList.contains('s')?'隐藏答案 ›':'查看答案 ›';
}
```

### Interview Accordion

```javascript
function tq(hdr){
  const iq=hdr.parentElement;
  iq.classList.toggle('o');
}
```

### Progress Tracking

```javascript
const cbs=document.querySelectorAll('.ci');
function upc(){
  const done=document.querySelectorAll('.ci:checked').length;
  document.getElementById('pc').textContent=done;
  cbs.forEach(cb=>{
    cb.closest('.pt').classList.toggle('done',cb.checked);
  });
  
  // Mark done dot
  if(done===10){
    document.querySelectorAll('.dot')[2].classList.add('d');
  }
}
cbs.forEach(cb=>cb.onchange=upc);
upc();

function mc(){
  cbs.forEach(cb=>cb.checked=true);
  upc();
}
```

---

## 🔤 Typography

**Fonts:**
```html
<link href="https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&display=swap" rel="stylesheet">
<link href="https://cdn.jsdelivr.net/npm/cn-fontsource-source-han-sans-sc-vf-ttf@1.0.4/font.css" rel="stylesheet">
```

**Font families:**
- **Body text:** `'Source Han Sans SC','Noto Sans SC',-apple-system,BlinkMacSystemFont,sans-serif`
- **Code/Numbers:** `'Space Mono',monospace`

**Font sizes:**
- H1 (header): `24px` (desktop), `19px` (mobile)
- H2 (section): `18px` (desktop), `16px` (mobile)
- H3 (subsection): `15px` (desktop), `14px` (mobile)
- Body: `13.5px` (p), `13px` (general)
- Code: `13px` (desktop), `11.5px` (mobile)
- Small text: `12px-12.5px`

**Font weights:**
- Regular: `400`
- Medium: `500`
- Bold: `600-700`

---

## 🎨 Color Palette Examples

**Stage 2 - Linux & Database:**

| Day | Color | Hex | Theme |
|-----|-------|-----|-------|
| Day 01 | Blue | `#3b82f6` | Linux基础 |
| Day 02 | Violet | `#8b5cf6` | 用户和权限 |
| Day 03 | Pink | `#ec4899` | MySQL安装 |
| Day 04 | Teal | `#0d9488` | SQL CRUD |
| Day 05 | Emerald | `#059669` | JOIN查询 |

---

## 📦 File Structure

```
outputs/
├── index.html                    # Main landing page
├── stage1/                       # QA基础知识 (Days 01-05)
│   ├── day01.html
│   ├── day02.html
│   ├── day03.html
│   ├── day04.html
│   └── day05.html
└── stage2/                       # Linux & Database
    ├── linux-day01.html
    ├── linux-day02.html
    ├── linux-day03.html
    ├── linux-day04.html
    └── linux-day05.html
```

**Mỗi file HTML:**
- Self-contained (không cần external CSS/JS files)
- Size: ~50-60 KB
- Lines: ~1,400-1,500 dòng code
- 5 panels/tabs cố định
- Full responsive design

---

## 🔗 Integration với index.html

**Curriculum entry format:**
```javascript
{
  id: 's2d5',                              // Unique ID
  day: 'Day 05',                           // Display label
  title: '连接查询、自关联、子查询',        // Title
  file: 'stage2/linux-day05.html',         // File path
  tags: 'JOIN·自关联·子查询',              // Tags
  practice: 10,                            // Number of exercises
  qa: 12,                                  // Number of interview Q&A
  status: 'available'                      // Status: available/locked/completed
}
```

**postMessage communication:**
```javascript
// From day file to index.html
window.parent.postMessage({
  type:'qa_quiz_result',
  score:12,
  total:15,
  day:'s2d5'
},'*');
```

---

## ✅ Quality Checklist

Mỗi file HTML mới cần đảm bảo:

**Structure:**
- [ ] Header với `.hd-tag`, `.hd-stats`
- [ ] Dots navigation (`#dots`)
- [ ] 5 tabs với emoji icons
- [ ] 5 panels tương ứng
- [ ] Navigation buttons cuối mỗi panel

**Content:**
- [ ] Panel 0: Lý thuyết với `.sec`, `.term`, `.box`
- [ ] Panel 1: Visualizations/demos
- [ ] Panel 2: Practice với checkbox tracking
- [ ] Panel 3: Interview Q&A với accordion
- [ ] Panel 4: Quiz 15 câu với auto-grading

**Styling:**
- [ ] CSS variables với `--day` color đúng
- [ ] All component classes có đầy đủ
- [ ] Syntax highlighting trong code blocks
- [ ] Responsive breakpoint `@media(max-width:640px)`
- [ ] Hover/active states đầy đủ

**Functionality:**
- [ ] `go(i)` function cho navigation
- [ ] `tr(el)` cho practice toggle
- [ ] `tq(hdr)` cho interview accordion
- [ ] `upc()` cho progress tracking
- [ ] `sub()` cho quiz submission
- [ ] postMessage API integration

**Testing:**
- [ ] All tabs clickable
- [ ] All navigation buttons work
- [ ] Practice checkboxes update progress
- [ ] Interview accordion expand/collapse
- [ ] Quiz submission shows results
- [ ] Mobile responsive verified

---

## 📚 Best Practices

**CSS:**
- Use CSS variables for theming
- Mobile-first approach
- Consistent spacing (multiples of 4px)
- Smooth transitions (0.2s default)

**HTML:**
- Semantic structure
- Self-contained (no external dependencies)
- Proper heading hierarchy (h1 > h2 > h3)
- Accessible form elements

**JavaScript:**
- Vanilla JS only (no frameworks)
- Event delegation where possible
- Clear function names
- Smooth scroll for navigation

**Content:**
- Clear, concise explanations
- Progressive difficulty
- Real-world examples
- Consistent terminology

---

## 🎯 Common Patterns

**Numbered sections:**
```html
<h2><span class="ico">1</span>Section Title</h2>
```

**Vertical bar subheadings:**
```html
<h3>Subsection Title</h3>
<!-- ::before pseudo-element adds bar -->
```

**Grid with numbered cards:**
```html
<div class="g3">
  <div class="card">
    <h4><span class="num">1</span>Card Title</h4>
    <p>Content...</p>
  </div>
</div>
```

**Code with syntax highlighting:**
```html
<div class="term">
<span class="k">SELECT</span> * <span class="k">FROM</span> table
<span class="k">WHERE</span> id <span class="o">=</span> <span class="n">1</span>;
</div>
```

**Inline code:**
```html
<code class="inl">SELECT * FROM students</code>
```

---

## 🚀 Development Workflow

1. **Đọc tài liệu gốc** (PDF/images)
2. **Chọn màu sắc** cho bài học (--day variable)
3. **Copy template** từ bài trước hoặc reference file
4. **Update header** (title, description, stats)
5. **Fill Panel 0** - Lý thuyết
6. **Fill Panel 1** - Visualizations
7. **Fill Panel 2** - Practice (10 bài)
8. **Fill Panel 3** - Interview (12 câu)
9. **Fill Panel 4** - Quiz (15 câu)
10. **Test all interactions**
11. **Verify mobile responsive**
12. **Update index.html curriculum**

---

## 📝 Notes

- File size thường ~50-60 KB (self-contained)
- Code ~1,400-1,500 dòng
- Quiz luôn 15 câu
- Practice thường 10 bài
- Interview Q&A: 10-12 câu
- Font CDN: Google Fonts + cn-fontsource
- No jQuery, no Bootstrap - Pure vanilla
- postMessage cho parent communication
- localStorage có thể dùng cho progress (nếu cần)

---

**Last updated:** May 2026
**Version:** 2.0
**Maintainer:** QA Learning Platform Team
