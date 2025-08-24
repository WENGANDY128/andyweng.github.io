# Eindeepseek 产品展示站点使用说明

## 1. 文件结构
- **首页**：`/index.html`
- **清单文件**：`/manifest.json`（放根目录，与 index.html 同级）
- **产品详情页**：`/products/product-1.html ~ product-6.html`
- **图片目录**：`/assets/imgs/product-N/1.png, 2.png, …`
- **参数/描述文本**（可选）：`/assets/txt/product-N/...`

## 2. 首页 index.html
- **逻辑改动**：
  - 首页只读取根目录的 `manifest.json`。
  - 每个产品只检测：`/assets/imgs/product-N/1.png`
    - **存在** → 渲染卡片（即使没有标题/描述）。
    - **不存在** → 跳过，不显示。
  - 保留了：GA4 统计、多语言切换（zh/en/de）、联系信息条、WhatsApp 群二维码卡片。

- **标题/描述规则**：
  - 优先用 `manifest.json` 的多语言 title。
  - 如果缺失，就用默认 `Product N`。
  - 描述可留空。

## 3. manifest.json
- 固定包含 1–6 条目（即使没有图片也保留，方便以后补图）。
- 格式示例：
  ```json
  {
    "items": [
      {
        "id": 1,
        "imgExt": "png",
        "title": { "zh": "产品 1", "en": "Product 1", "de": "Produkt 1" }
      },
      {
        "id": 2,
        "imgExt": "png",
        "title": { "zh": "产品 2", "en": "Product 2", "de": "Produkt 2" }
      }
    ]
  }
  ```

## 4. 产品详情页（product-1 ~ product-6.html）
- 已统一修改为：  
  - **只加载 PNG**（固定路径 `/assets/imgs/product-N/1.png ~ 12.png`）。
  - 遍历时遇到缺图就停止。
  - 保证加载速度快、逻辑统一。

## 5. 使用方式
1. 确保每个要展示的产品目录下有一张首图：
   ```
   /assets/imgs/product-1/1.png
   /assets/imgs/product-2/1.png
   ...
   ```
   没有 `1.png` 的产品将不会显示。

2. 确保根目录有最新的：
   - `index.html`（新版逻辑）
   - `manifest.json`（包含 1–6 条目）

3. 详情页 `/products/product-N.html` 已经更新为只读 PNG。

4. 如果你上传了新产品：
   - 新建 `/assets/imgs/product-7/1.png`
   - 在 manifest.json 里增加 id=7 的条目
   - 首页就会自动显示。

## 6. 检查方法
- 打开：`https://www.eindeepseek.com/manifest.json` → 确认有 1–6 项。
- 打开：`https://www.eindeepseek.com/assets/imgs/product-1/1.png` → 确认图片能显示。
- 打开首页：`https://www.eindeepseek.com/` → 应该只显示有图片的产品。

---

✅ **总结**：  
首页和详情页都统一为 PNG，只要 `/assets/imgs/product-N/1.png` 存在就显示产品，否则隐藏；manifest.json 永远保留 1–6（以后可扩展），不需要改 index.html。
