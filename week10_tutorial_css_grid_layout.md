# คู่มือปฏิบัติการ Week 10: การจัดโครงสร้างเลย์เอาต์ขั้นสูงด้วย CSS Grid Layout

ในสัปดาห์นี้เราจะต่อยอดจาก CSS Flexbox ด้วย **CSS Grid Layout** ซึ่งเป็นระบบจัดเลย์เอาต์แบบ 2 มิติ ช่วยให้เราวางองค์ประกอบเป็นแถวและคอลัมน์พร้อมกันได้อย่างเป็นระบบ เหมาะกับหน้าแกลเลอรี แดชบอร์ด หน้ารายการสินค้า และโครงหน้าจอที่มีองค์ประกอบหลายขนาด

---

## 1. แนวคิด Grid Container และ Grid Items

CSS Grid แบ่งบทบาทขององค์ประกอบออกเป็น 2 ส่วน:

1. **Grid Container (กล่องแม่):** กล่อง HTML ที่เปิดใช้คำสั่ง `display: grid;` เพื่อสร้างตารางสำหรับจัดวางเนื้อหาภายใน
2. **Grid Items (กล่องลูก):** องค์ประกอบที่เป็นลูกโดยตรงของ Grid Container ซึ่งจะถูกวางลงในช่องตาราง (Grid Cell) โดยอัตโนมัติ

```html
<!-- โครงสร้างใน HTML -->
<section class="gallery-grid">
    <article class="gallery-item">ภาพที่ 1</article>
    <article class="gallery-item">ภาพที่ 2</article>
    <article class="gallery-item">ภาพที่ 3</article>
</section>
```

```css
.gallery-grid {
    display: grid;
}
```

### Grid ต่างจาก Flexbox อย่างไร?

* **Flexbox** เป็นเลย์เอาต์แบบ 1 มิติ เหมาะกับการจัดเรียงในแนวเดียว เช่น เมนูนำทาง แถวปุ่ม หรือการ์ดสินค้าในหนึ่งแถว
* **Grid** เป็นเลย์เอาต์แบบ 2 มิติ เหมาะกับการกำหนดทั้งจำนวนคอลัมน์ จำนวนแถว และตำแหน่งของแต่ละชิ้นงานพร้อมกัน

แนวทางเลือกใช้ที่จำง่ายคือ: ใช้ **Flexbox** สำหรับการจัดส่วนย่อยในแนวเดียว และใช้ **Grid** เมื่อออกแบบโครงสร้างหลักที่มีแถวและคอลัมน์ชัดเจน

---

## 2. สร้างตารางด้วย Grid Container

### 2.1 `display: grid;`

ใช้กับกล่องแม่เพื่อเริ่มต้นระบบ CSS Grid

```css
.dashboard {
    display: grid;
}
```

### 2.2 `grid-template-columns` (กำหนดคอลัมน์)

กำหนดจำนวนและขนาดคอลัมน์ โดยสามารถใช้หน่วย `px`, `%`, `auto` และ `fr` ได้

```css
.dashboard {
    display: grid;
    grid-template-columns: 240px 1fr 1fr;
}
```

ในตัวอย่างนี้ คอลัมน์แรกกว้าง 240 พิกเซล ส่วนอีก 2 คอลัมน์แบ่งพื้นที่ที่เหลือเท่า ๆ กันด้วยหน่วย `fr` (fraction)

* `1fr` หมายถึง 1 ส่วนของพื้นที่ว่างที่เหลือ
* `2fr` หมายถึง 2 ส่วน จึงกว้างเป็น 2 เท่าของ `1fr`

```css
.layout {
    display: grid;
    grid-template-columns: 1fr 2fr 1fr;
}
```

### 2.3 `repeat()` (เขียนรูปแบบซ้ำให้สั้นลง)

เมื่อคอลัมน์มีขนาดซ้ำกัน ใช้ `repeat()` เพื่อลดการเขียนโค้ดซ้ำ

```css
.product-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
}
```

คำสั่งนี้ให้ผลเหมือน `1fr 1fr 1fr` คือสร้าง 3 คอลัมน์ที่กว้างเท่ากัน

### 2.4 `grid-template-rows` (กำหนดแถว)

กำหนดความสูงของแถวเมื่อหน้าเว็บต้องการโครงสร้างแน่นอน เช่น ส่วนหัว เนื้อหา และส่วนท้าย

```css
.page-layout {
    display: grid;
    grid-template-rows: auto 1fr auto;
    min-height: 100vh;
}
```

ค่า `auto 1fr auto` เหมาะกับหน้าเว็บทั่วไป เพราะส่วนหัวและส่วนท้ายสูงตามเนื้อหา ส่วนเนื้อหาหลักขยายกินพื้นที่ว่างที่เหลือ

### 2.5 `gap` (ระยะห่างระหว่างช่อง)

ใช้กำหนดช่องว่างระหว่าง Grid Items โดยไม่ต้องใส่ `margin` ให้แต่ละกล่อง

```css
.product-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
}
```

หากต้องการกำหนดระยะห่างแถวและคอลัมน์ต่างกัน ให้ใช้ `row-gap` และ `column-gap`

```css
.schedule-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    row-gap: 16px;
    column-gap: 24px;
}
```

---

## 3. ควบคุมตำแหน่งและขนาดของ Grid Items

### 3.1 `grid-column` (กินพื้นที่ตามแนวคอลัมน์)

ใช้ให้ Grid Item ขยายข้ามหลายคอลัมน์ด้วยคำว่า `span`

```css
.featured-item {
    grid-column: span 2;
}
```

คำสั่งนี้ทำให้กล่อง `.featured-item` กว้างเท่ากับ 2 คอลัมน์ เหมาะสำหรับภาพเด่นหรือการ์ดข้อมูลสำคัญ

นอกจากนี้ยังระบุตำแหน่งด้วยเส้นกริดได้ เช่น `grid-column: 1 / 3;` หมายถึงเริ่มที่เส้นคอลัมน์ 1 และสิ้นสุดก่อนเส้นคอลัมน์ 3

### 3.2 `grid-row` (กินพื้นที่ตามแนวแถว)

ใช้กำหนดให้กล่องขยายข้ามหลายแถว

```css
.tall-item {
    grid-row: span 2;
}
```

เมื่อนำ `grid-column` และ `grid-row` มาใช้ร่วมกัน เราสามารถสร้างแกลเลอรีที่มีภาพเล็ก ภาพกว้าง และภาพสูงสลับกันได้

```css
.hero-item {
    grid-column: span 2;
    grid-row: span 2;
}
```

### 3.3 `grid-template-areas` (ตั้งชื่อพื้นที่เลย์เอาต์)

สำหรับหน้าเว็บที่มีโครงสร้างชัดเจน สามารถตั้งชื่อแต่ละพื้นที่เพื่อให้อ่าน CSS ได้ง่าย

```css
.site-layout {
    display: grid;
    grid-template-columns: 220px 1fr;
    grid-template-areas:
        "header header"
        "sidebar main"
        "footer footer";
    gap: 20px;
}

header  { grid-area: header; }
aside   { grid-area: sidebar; }
main    { grid-area: main; }
footer  { grid-area: footer; }
```

---

## 4. สร้าง Gallery Grid ที่ยืดหยุ่นตามหน้าจอ

การใช้ `repeat()`, `auto-fit` และ `minmax()` ทำให้จำนวนคอลัมน์เปลี่ยนตามพื้นที่หน้าจอโดยอัตโนมัติ

```css
.gallery-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    grid-auto-rows: 160px;
    gap: 16px;
}
```

ความหมายของคำสั่งนี้:

* `auto-fit` ให้เบราว์เซอร์คำนวณจำนวนคอลัมน์ที่วางได้เอง
* `minmax(180px, 1fr)` ให้แต่ละคอลัมน์แคบสุด 180px และขยายได้ถึงส่วนที่เหลือ
* `grid-auto-rows: 160px` กำหนดความสูงพื้นฐานของแถวที่สร้างเพิ่มอัตโนมัติ

ตัวอย่าง CSS ฉบับสมบูรณ์สำหรับภาพในแกลเลอรี:

```css
* {
    box-sizing: border-box;
}

.gallery-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    grid-auto-rows: 160px;
    gap: 16px;
    max-width: 1100px;
    margin: 40px auto;
    padding: 0 20px;
}

.gallery-item {
    overflow: hidden;
    border-radius: 12px;
    box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
}

.gallery-item img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
}

.gallery-item--wide {
    grid-column: span 2;
}

.gallery-item--tall {
    grid-row: span 2;
}

.gallery-item--featured {
    grid-column: span 2;
    grid-row: span 2;
}
```

> ข้อควรระวัง: เมื่อหน้าจอแคบมาก ควรยกเลิกการกินหลายช่องของภาพเด่น เพื่อไม่ให้เกิดการเลื่อนแนวนอน

```css
@media (max-width: 480px) {
    .gallery-item--wide,
    .gallery-item--tall,
    .gallery-item--featured {
        grid-column: span 1;
        grid-row: span 1;
    }
}
```

---

## 5. ตัวอย่างโครงสร้าง HTML สำหรับ Gallery Grid

```html
<main class="gallery-grid">
    <article class="gallery-item gallery-item--featured">
        <img src="images/featured.jpg" alt="ภาพทิวทัศน์เด่น">
    </article>

    <article class="gallery-item gallery-item--wide">
        <img src="images/city.jpg" alt="ภาพเมืองยามเย็น">
    </article>

    <article class="gallery-item gallery-item--tall">
        <img src="images/portrait.jpg" alt="ภาพบุคคล">
    </article>

    <article class="gallery-item">
        <img src="images/food.jpg" alt="ภาพอาหาร">
    </article>

    <article class="gallery-item">
        <img src="images/nature.jpg" alt="ภาพธรรมชาติ">
    </article>
</main>
```

ควรเขียนค่า `alt` ให้บอกความหมายของภาพอย่างกระชับเสมอ เพื่อช่วยให้ผู้ใช้โปรแกรมอ่านหน้าจอเข้าใจเนื้อหา และช่วยให้หน้าเว็บมีความหมายแม้รูปภาพโหลดไม่สำเร็จ

---

## 📝 ใบงานปฏิบัติท้ายบทเรียน (Assignment 10)

**โจทย์:** ให้นักเรียนสร้างหน้าแกลเลอรีรูปภาพชื่อ `gallery-grid.html` โดยมีคุณสมบัติดังนี้:

1. **โครงสร้างแกลเลอรี:**
   * สร้าง Grid Container สำหรับเก็บภาพอย่างน้อย 8 ภาพ
   * ใช้ `display: grid;`, `gap` และ `grid-template-columns` จัดวางภาพ
   * ใช้ `repeat(auto-fit, minmax(...))` เพื่อให้จำนวนคอลัมน์ปรับตามความกว้างของหน้าจอ

2. **ภาพเด่นและภาพหลายขนาด:**
   * กำหนดภาพเด่นอย่างน้อย 1 ภาพให้กินพื้นที่ 2 คอลัมน์และ 2 แถวด้วย `grid-column: span 2;` และ `grid-row: span 2;`
   * กำหนดภาพแบบกว้างหรือสูงเพิ่มเติมอย่างน้อย 2 ภาพ เพื่อให้เกิดจังหวะของเลย์เอาต์ที่หลากหลาย

3. **การตกแต่งและการเข้าถึง:**
   * ใช้ทักษะ CSS Box Model จากสัปดาห์ที่ 6 ตกแต่งกรอบ ขอบมน และเงาให้ภาพแต่ละใบ
   * กำหนด `object-fit: cover;` เพื่อให้ภาพเต็มกรอบโดยไม่เสียสัดส่วน
   * ใส่ข้อความ `alt` ที่อธิบายภาพทุกภาพ

4. **ทดสอบ Responsive Layout:**
   * ย่อและขยายหน้าต่างเบราว์เซอร์เพื่อตรวจสอบว่ากริดเพิ่มหรือลดจำนวนคอลัมน์ได้โดยไม่เกิดแถบเลื่อนแนวนอน
   * ที่หน้าจอขนาดไม่เกิน 480px ให้ภาพที่เคยกินหลายช่องกลับมาแสดงขนาดปกติ 1 ช่อง
