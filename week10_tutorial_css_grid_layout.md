# คู่มือปฏิบัติการ Week 10: การจัดโครงสร้างเลย์เอาต์ขั้นสูงด้วย CSS Grid Layout

บทเรียนนี้สอน CSS Grid แบบค่อยเป็นค่อยไป: เริ่มจาก HTML ที่มีอยู่ เปิด Grid ด้วยคำสั่งเดียว ดูผลลัพธ์ แล้วจึงเพิ่มคอลัมน์ แถว ระยะห่าง การขยายพื้นที่ และการรองรับมือถือทีละขั้น

> วิธีเรียนที่แนะนำ: คัดลอกโค้ดแต่ละขั้นลงในไฟล์เดียวกัน กดบันทึก และดูผลในเบราว์เซอร์ก่อนก้าวไปยังขั้นถัดไป อย่าข้ามขั้น เพราะคำสั่งใหม่ทุกคำสั่งต่อยอดจากคำสั่งก่อนหน้า

---

## 0. เตรียมไฟล์สำหรับทดลอง

สร้างโฟลเดอร์ `week10-grid` แล้วสร้างไฟล์ชื่อ `grid-demo.html` ภายใน VS Code จากนั้นวางโค้ดตั้งต้นนี้

```html
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ทดลอง CSS Grid</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>แกลเลอรีของฉัน</h1>

    <section class="gallery">
        <article class="item">1</article>
        <article class="item">2</article>
        <article class="item">3</article>
        <article class="item">4</article>
        <article class="item">5</article>
        <article class="item">6</article>
    </section>
</body>
</html>
```

สร้างไฟล์ `style.css` ในโฟลเดอร์เดียวกัน แล้วใส่ CSS สำหรับทำให้ผลลัพธ์มองเห็นง่ายก่อน

```css
* {
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 32px;
    background: #f4f4f4;
}

.item {
    padding: 28px;
    background: #333;
    color: white;
    font-size: 24px;
    text-align: center;
}
```

ตอนนี้ `.item` ทั้ง 6 กล่องจะเรียงจากบนลงล่างตามพฤติกรรมปกติของ `<article>` เราจะเริ่มเปลี่ยนเลย์เอาต์จากกล่องแม่ `.gallery`

---

## 1. CSS Grid คืออะไร?

CSS Grid คือระบบจัดเลย์เอาต์แบบ **2 มิติ** หมายความว่าเราสามารถควบคุมได้พร้อมกันทั้ง:

* **คอลัมน์ (Columns):** แนวตั้งจากซ้ายไปขวา
* **แถว (Rows):** แนวนอนจากบนลงล่าง

เปรียบเทียบกับ Flexbox ที่เรียนในสัปดาห์ 7:

| เลือกใช้ | เหมาะกับงาน |
| --- | --- |
| Flexbox | จัดของในแนวเดียวเป็นหลัก เช่น เมนู ปุ่ม หรือแถวข้อมูล |
| CSS Grid | วางของหลายแถวและหลายคอลัมน์ เช่น แกลเลอรี แดชบอร์ด หรือหน้าเว็บทั้งหน้า |

คำศัพท์ที่ควรจำ:

1. **Grid Container:** กล่องแม่ที่เราเปิด `display: grid;`
2. **Grid Item:** ลูกโดยตรงของ Grid Container
3. **Grid Track:** แถวหรือคอลัมน์หนึ่งเส้นในกริด
4. **Grid Cell:** ช่องหนึ่งช่องที่เกิดจากแถวตัดกับคอลัมน์
5. **Grid Line:** เส้นแบ่งขอบของแถวและคอลัมน์ ใช้อ้างอิงตำแหน่งได้

ในตัวอย่างนี้ `.gallery` คือ Grid Container และ `.item` ทั้ง 6 กล่องคือ Grid Items

---

## 2. ขั้นที่ 1: เปิดใช้ Grid ด้วย `display: grid`

เพิ่มคำสั่งนี้ใน `style.css`

```css
.gallery {
    display: grid;
}
```

`display: grid;` บอกเบราว์เซอร์ว่า “กล่องนี้เป็น Grid Container” ลูกโดยตรงทุกตัวจึงกลายเป็น Grid Item อัตโนมัติ

แต่ถ้ายังไม่กำหนดคอลัมน์ ผลลัพธ์จะยังดูคล้ายเดิม: แต่ละ Item ใช้พื้นที่เต็มแถวและเรียงจากบนลงล่าง นี่เป็นจุดเริ่มต้นที่ถูกต้อง เพราะเราเพิ่งเปิดระบบ Grid เท่านั้น

> จำไว้: ต้องใส่ `display: grid;` ที่ **กล่องแม่** ไม่ใช่ที่กล่องลูก หากใส่ที่ `.item` แต่ละใบ จะไม่ได้สร้างแกลเลอรีร่วมกัน

---

## 3. ขั้นที่ 2: สร้างคอลัมน์ด้วย `grid-template-columns`

เพิ่มคอลัมน์ 3 ช่องที่กว้างเท่ากัน

```css
.gallery {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
}
```

### `fr` คืออะไร?

`fr` ย่อมาจาก **fraction** หรือ “ส่วนแบ่งของพื้นที่ว่าง”

* `1fr 1fr 1fr` แบ่งพื้นที่ว่างเป็น 3 ส่วนเท่ากัน
* `1fr 2fr` แบ่งพื้นที่ว่างเป็น 3 ส่วน โดยคอลัมน์แรกได้ 1 ส่วน และคอลัมน์ที่สองได้ 2 ส่วน

ลองเปลี่ยนเป็นตัวอย่างนี้ แล้วสังเกตว่ากล่องที่สองกว้างเป็นสองเท่าของกล่องแรกและกล่องที่สาม

```css
.gallery {
    display: grid;
    grid-template-columns: 1fr 2fr 1fr;
}
```

นอกจาก `fr` เรายังใช้หน่วยอื่นได้ เช่น

```css
.layout {
    display: grid;
    grid-template-columns: 220px 1fr 160px;
}
```

ตัวอย่างนี้เหมาะกับหน้าเว็บที่มี Sidebar ซ้ายกว้างคงที่ 220px เนื้อหากลางยืดหยุ่น และแถบด้านขวากว้างคงที่ 160px

---

## 4. ขั้นที่ 3: เขียนคอลัมน์ซ้ำให้สั้นด้วย `repeat()`

โค้ด `1fr 1fr 1fr` ใช้งานได้ แต่จะยาวหากมี 4 หรือ 6 คอลัมน์ ใช้ `repeat()` แทนได้ดังนี้

```css
.gallery {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
}
```

รูปแบบคำสั่งคือ `repeat(จำนวนครั้ง, ขนาดคอลัมน์)`

```css
grid-template-columns: repeat(4, 1fr);   /* 4 คอลัมน์เท่ากัน */
grid-template-columns: repeat(2, 200px); /* 2 คอลัมน์ กว้างช่องละ 200px */
```

สำหรับแกลเลอรีของเรา ใช้ `repeat(3, 1fr)` เพื่อสร้าง 3 คอลัมน์ก่อน เมื่อมี Item เกิน 3 ชิ้น Grid จะนำชิ้นที่เหลือขึ้นแถวใหม่เองโดยอัตโนมัติ

---

## 5. ขั้นที่ 4: กำหนดแถวด้วย `grid-template-rows`

โดยทั่วไป Grid สร้างแถวเพิ่มให้เองตามจำนวน Item แต่เรากำหนดความสูงของแถวได้เมื่อเลย์เอาต์ต้องการความสูงที่แน่นอน

```css
.gallery {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: 160px 160px;
}
```

โค้ดนี้สร้าง 2 แถว แถวละ 160px หากมี Item มากกว่า 6 ชิ้น แถวที่เพิ่มอัตโนมัติอาจสูงตามเนื้อหา

ถ้าต้องการกำหนดความสูงมาตรฐานสำหรับแถวที่ Grid สร้างเพิ่มเอง ให้ใช้ `grid-auto-rows`

```css
.gallery {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-auto-rows: 160px;
}
```

คำสั่งนี้เหมาะกับ Gallery เพราะภาพทุกช่องเริ่มต้นด้วยความสูงเท่ากัน

---

## 6. ขั้นที่ 5: เว้นระยะห่างด้วย `gap`

ถ้ากล่องติดกันจนเกินไป ให้เพิ่ม `gap` ที่ Grid Container

```css
.gallery {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-auto-rows: 160px;
    gap: 16px;
}
```

`gap: 16px;` เพิ่มช่องว่าง 16px ทั้งระหว่างแถวและระหว่างคอลัมน์ โดยไม่ต้องใส่ `margin` ให้ Item ทีละกล่อง

หากต้องการระยะห่างคนละค่า ใช้ได้ 2 แบบ

```css
gap: 12px 24px;       /* แถว 12px, คอลัมน์ 24px */
row-gap: 12px;
column-gap: 24px;
```

> ใช้ `gap` เป็นตัวเลือกแรกสำหรับระยะห่างใน Grid เพราะคำนวณพื้นที่ได้ง่ายและไม่ทำให้ขอบด้านนอกของกริดเกินมา

---

## 7. ขั้นที่ 6: อ้างอิงตำแหน่งด้วย Grid Line

Grid 3 คอลัมน์มีเส้นกริดแนวตั้ง 4 เส้น: เส้น 1 อยู่ก่อนคอลัมน์แรก และเส้น 4 อยู่หลังคอลัมน์สุดท้าย

ให้กล่องแรกกินตั้งแต่เส้น 1 ถึงก่อนเส้น 3:

```css
.item:first-child {
    grid-column: 1 / 3;
}
```

ผลคือ Item แรกกินพื้นที่คอลัมน์ที่ 1 และ 2 ส่วน Item อื่นจะถูก Grid จัดตำแหน่งใหม่ให้อัตโนมัติ

รูปแบบของคำสั่งคือ

```css
grid-column: จุดเริ่มต้น / จุดสิ้นสุด;
grid-row: จุดเริ่มต้น / จุดสิ้นสุด;
```

ตัวอย่างให้กล่องหนึ่งเริ่มที่คอลัมน์ 2 และกว้างถึงขอบสุดท้ายของ Grid:

```css
.notice {
    grid-column: 2 / 4;
}
```

---

## 8. ขั้นที่ 7: ขยายกล่องด้วย `span`

การนับเส้น Grid มีประโยชน์ แต่สำหรับผู้เริ่มต้น `span` อ่านง่ายกว่า เพราะหมายถึง “กินกี่ช่อง”

```css
.item--wide {
    grid-column: span 2;
}
```

Item ที่มีคลาส `item--wide` จะกินพื้นที่ 2 คอลัมน์

```css
.item--tall {
    grid-row: span 2;
}
```

Item ที่มีคลาส `item--tall` จะกินพื้นที่ 2 แถว

นำมาใช้พร้อมกันเพื่อสร้างภาพเด่นขนาดใหญ่

```css
.item--featured {
    grid-column: span 2;
    grid-row: span 2;
}
```

เพิ่มคลาสลงใน HTML ดังนี้

```html
<article class="item item--featured">ภาพเด่น</article>
<article class="item item--wide">ภาพกว้าง</article>
<article class="item item--tall">ภาพสูง</article>
```

---

## 9. ตั้งชื่อพื้นที่หน้าเว็บด้วย `grid-template-areas`

ใช้เมื่อหน้าเว็บมีส่วนหัว แถบข้าง เนื้อหาหลัก และส่วนท้าย ทำให้ CSS อ่านเหมือนแผนผัง

```css
.page {
    display: grid;
    grid-template-columns: 220px 1fr;
    grid-template-areas:
        "header  header"
        "sidebar main"
        "footer  footer";
    gap: 20px;
}

header { grid-area: header; }
aside  { grid-area: sidebar; }
main   { grid-area: main; }
footer { grid-area: footer; }
```

ข้อความแต่ละบรรทัดใน `grid-template-areas` แทน 1 แถว และชื่อที่ซ้ำกันหมายถึงพื้นที่เดียวกันที่ต่อเนื่องกัน

---

## 10. ขั้นที่ 8: สร้างคอลัมน์ที่ยืดหยุ่นด้วย `minmax()`

การกำหนด `repeat(3, 1fr)` ทำให้มี 3 คอลัมน์เสมอ ซึ่งอาจแคบเกินไปบนมือถือ แก้ด้วย `minmax()`

```css
.gallery {
    display: grid;
    grid-template-columns: repeat(3, minmax(180px, 1fr));
    gap: 16px;
}
```

`minmax(180px, 1fr)` หมายถึง คอลัมน์หนึ่งช่องมีความกว้างอย่างน้อย 180px และสามารถขยายได้จนถึง 1 ส่วนของพื้นที่ที่เหลือ

แต่ตัวอย่างนี้ยังพยายามรักษา 3 คอลัมน์อยู่เสมอ ขั้นถัดไปจะให้เบราว์เซอร์เลือกจำนวนคอลัมน์ที่พอดีเอง

---

## 11. ขั้นที่ 9: ให้ Grid คำนวณคอลัมน์เองด้วย `auto-fit`

เปลี่ยนจำนวนครั้งใน `repeat()` เป็น `auto-fit`

```css
.gallery {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    grid-auto-rows: 160px;
    gap: 16px;
}
```

อ่านคำสั่งนี้เป็นประโยคได้ว่า:

> “สร้างคอลัมน์ได้มากเท่าที่พื้นที่พอ โดยแต่ละคอลัมน์ต้องไม่แคบกว่า 180px และให้ขยายเพื่อเติมพื้นที่ที่เหลือ”

เมื่อหน้าจอกว้าง Grid อาจมี 4 หรือ 5 คอลัมน์ เมื่อหน้าจอแคบลง Grid ลดเป็น 2 หรือ 1 คอลัมน์โดยอัตโนมัติ จึงเป็นคำสั่งสำคัญสำหรับ Responsive Gallery

---

## 12. ขั้นที่ 10: ปรับภาพเด่นสำหรับมือถือด้วย Media Query

แม้จำนวนคอลัมน์จะยืดหยุ่นแล้ว แต่ Item ที่กิน 2 แถวหรือ 2 คอลัมน์อาจใหญ่เกินไปบนมือถือ จึงกำหนดกติกาเพิ่มเติม

```css
@media (max-width: 480px) {
    .item--wide,
    .item--tall,
    .item--featured {
        grid-column: span 1;
        grid-row: span 1;
    }
}
```

เมื่อหน้าจอกว้างไม่เกิน 480px ภาพทุกใบจะกลับมากินพื้นที่ปกติ 1 ช่อง จึงลดโอกาสเกิดแถบเลื่อนแนวนอนและช่วยให้เลย์เอาต์อ่านง่าย

---

## 13. โค้ด Gallery Grid ฉบับสมบูรณ์

### HTML

```html
<main class="gallery">
    <article class="item item--featured">
        <img src="images/featured.jpg" alt="ภาพทิวทัศน์เด่น">
    </article>
    <article class="item item--wide">
        <img src="images/city.jpg" alt="ภาพเมืองยามเย็น">
    </article>
    <article class="item item--tall">
        <img src="images/portrait.jpg" alt="ภาพบุคคล">
    </article>
    <article class="item"><img src="images/food.jpg" alt="ภาพอาหาร"></article>
    <article class="item"><img src="images/nature.jpg" alt="ภาพธรรมชาติ"></article>
    <article class="item"><img src="images/work.jpg" alt="พื้นที่ทำงาน"></article>
    <article class="item"><img src="images/sea.jpg" alt="ภาพทะเล"></article>
    <article class="item"><img src="images/cafe.jpg" alt="ภาพคาเฟ่"></article>
</main>
```

### CSS

```css
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    padding: 32px;
    font-family: Arial, sans-serif;
    background: #f4f4f4;
}

.gallery {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    grid-auto-rows: 160px;
    gap: 16px;
    max-width: 1100px;
    margin: 0 auto;
}

.item {
    overflow: hidden;
    border-radius: 12px;
    box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
}

.item img {
    display: block;
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.item--wide { grid-column: span 2; }
.item--tall { grid-row: span 2; }
.item--featured {
    grid-column: span 2;
    grid-row: span 2;
}

@media (max-width: 480px) {
    body { padding: 16px; }
    .item--wide,
    .item--tall,
    .item--featured {
        grid-column: span 1;
        grid-row: span 1;
    }
}
```

### `object-fit: cover` สำคัญอย่างไร?

ภาพแต่ละรูปมีสัดส่วนไม่เท่ากัน แต่ Grid Item ของเราอาจเป็นสี่เหลี่ยมจัตุรัส สี่เหลี่ยมกว้าง หรือสี่เหลี่ยมสูง `object-fit: cover;` ทำให้ภาพเต็มกรอบโดยรักษาสัดส่วนเดิมไว้ ส่วนที่เกินจากกรอบจะถูกตัดออกอย่างพอดี

---

## 14. ตรวจงานและแก้ปัญหาที่พบบ่อย

### กล่องไม่เรียงเป็นกริด

ตรวจว่าคุณใส่ `display: grid;` ที่กล่องแม่ และ `.item` เป็นลูกโดยตรงของ `.gallery`

### คอลัมน์ล้นหน้าจอ

หลีกเลี่ยงคอลัมน์กว้างตายตัวหลายช่อง เช่น `repeat(4, 300px)` บนมือถือ ให้ใช้ `repeat(auto-fit, minmax(180px, 1fr))` แทน

### ภาพบิดหรือไม่เต็มช่อง

ตรวจว่า `<img>` มีทั้ง `width: 100%;`, `height: 100%;` และ `object-fit: cover;`

### ช่องว่างแปลก ๆ ระหว่างกล่อง

ใช้ `gap` ที่ Grid Container แทนการใส่ `margin` รอบ Item ทุกชิ้น และตรวจว่ากล่องลูกไม่ได้มี `margin` เหลืออยู่

### ภาพเด่นใหญ่เกินบนมือถือ

เพิ่ม Media Query เพื่อคืนค่า `grid-column` และ `grid-row` เป็น `span 1`

---

## 📝 ใบงานปฏิบัติท้ายบทเรียน (Assignment 10)

**โจทย์:** สร้างหน้าแกลเลอรีรูปภาพชื่อ `gallery-grid.html` โดยใช้ความรู้จากบทเรียนนี้

1. **เริ่มต้นให้ถูกโครงสร้าง**
   * สร้าง Grid Container 1 กล่อง และมี Grid Items อย่างน้อย 8 กล่อง
   * ใส่ภาพจริงในแต่ละ Item พร้อมข้อความ `alt` ที่อธิบายภาพ

2. **สร้างกริดที่ตอบสนองต่อหน้าจอ**
   * ใช้ `display: grid;`, `gap` และ `grid-auto-rows`
   * ใช้ `grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));`

3. **สร้างจังหวะภาพหลายขนาด**
   * สร้างภาพเด่น 1 ภาพด้วย `grid-column: span 2;` และ `grid-row: span 2;`
   * สร้างภาพกว้างและภาพสูงอย่างละอย่างน้อย 1 ภาพ

4. **รองรับมือถือ**
   * ใช้ Media Query ที่ `max-width: 480px`
   * ยกเลิกการกินหลายช่องของภาพเด่นด้วย `span 1`
   * ทดลองย่อและขยายเบราว์เซอร์ แล้วถ่ายภาพหน้าจอ Desktop และ Mobile ส่งพร้อมงาน

5. **เกณฑ์ตรวจงาน**
   * ไม่มีแถบเลื่อนแนวนอนในหน้าจอมือถือ
   * ภาพเต็มกรอบ ไม่บิด และมีระยะห่างสม่ำเสมอ
   * โค้ดจัดย่อหน้าอ่านง่าย และตั้งชื่อคลาสสื่อความหมาย

---

## อ่านต่อและฝึกเพิ่ม

โครงบทเรียนนี้เรียงคำสั่งจากตัวอย่างสั้นไปสู่ชิ้นงานจริงในแนวเดียวกับ [CSS Grid Layout ของ W3Schools](https://www.w3schools.com/css/css_grid.asp) ซึ่งมีแบบฝึกหัดและตัวอย่างเพิ่มเติมสำหรับทดลองด้วยตนเอง
