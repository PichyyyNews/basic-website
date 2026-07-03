# คู่มือปฏิบัติการ Week 4: การจัดตารางขั้นสูง และแบบฟอร์มเบื้องต้น

ในสัปดาห์นี้เราจะเริ่มศึกษาทักษะการจัดแต่งข้อมูลตารางที่ซับซ้อนขึ้นด้วยการยุบรวมเซลล์ เรียนรู้วิธีใส่สไตล์ตารางข้อมูลที่สวยงาม และศึกษาโครงสร้างการสร้างฟอร์มเพื่อรับข้อมูลต่างๆ จากผู้ใช้ตามมาตรฐานเว็บสากล

---

## 1. การรวมเซลล์ตารางด้วย colspan และ rowspan
ในทางปฏิบัติของตารางข้อมูล มักมีหัวข้อหลักที่ครอบคลุมหัวข้อย่อยด้านล่าง เราจะใช้คุณสมบัติดังนี้:
*   `colspan="จำนวน"`: ขยายคอลัมน์ออกไปด้านขวา ( Merge Cells แนวนอน)
*   `rowspan="จำนวน"`: ขยายแถวลงไปด้านล่าง ( Merge Cells แนวตั้ง)

### ตัวอย่างโค้ดตารางคะแนน:
```html
<table style="width: 100%; border: 1px solid black; border-collapse: collapse;">
    <thead>
        <tr style="background-color: #ddd;">
            <th rowspan="2" style="border: 1px solid black; padding: 8px;">รายชื่อ</th>
            <th colspan="2" style="border: 1px solid black; padding: 8px;">คะแนนสอบกลางภาค</th>
            <th colspan="2" style="border: 1px solid black; padding: 8px;">คะแนนสอบปลายภาค</th>
        </tr>
        <tr style="background-color: #eee;">
            <th style="border: 1px solid black; padding: 8px;">ตอนที่ 1</th>
            <th style="border: 1px solid black; padding: 8px;">ตอนที่ 2</th>
            <th style="border: 1px solid black; padding: 8px;">ตอนที่ 1</th>
            <th style="border: 1px solid black; padding: 8px;">ตอนที่ 2</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black; padding: 8px; text-align: center;">สมชาย มั่นใจ</td>
            <td style="border: 1px solid black; padding: 8px; text-align: center;">20</td>
            <td style="border: 1px solid black; padding: 8px; text-align: center;">18</td>
            <td style="border: 1px solid black; padding: 8px; text-align: center;">25</td>
            <td style="border: 1px solid black; padding: 8px; text-align: center;">22</td>
        </tr>
    </tbody>
</table>
```

---

## 2. ตารางรวมคุณสมบัติ CSS สำหรับแต่งตารางข้อมูล

| CSS Property | หน้าที่การทำงาน | ตัวอย่างรูปแบบการกำหนดค่า |
|---|---|---|
| `border-collapse` | รวมขอบของตารางและช่องเซลล์ให้เป็นเส้นเดียว | `border-collapse: collapse;` (ใช้ประจำ) |
| `border-spacing` | กำหนดระยะห่างระหว่างช่องเซลล์ตาราง | `border-spacing: 5px;` |
| `vertical-align` | จัดข้อความให้อยู่บน กลาง หรือล่างของเซลล์ | `vertical-align: middle;` (top, middle, bottom) |
| `tr:nth-child(even)` | เลือกทุกๆ แถวคู่เพื่อทำสลับสี (Zebra Stripes) | `background-color: #f2f2f2;` |

---

## 3. โครงสร้างและการรับข้อมูลด้วย HTML Forms

แบบฟอร์มประกาศขึ้นมาโดยการใช้แท็กคู่ `<form>` พร้อมกำหนดตัวแปรกำกับทิศทางส่งข้อมูล:
```html
<form action="submit.php" method="POST">
    <!-- กล่องคอนโทรลต่างๆ จะอยู่ด้านในนี้ -->
</form>
```

### 3.1 แท็กป้อนอินพุต `<input>` และ Attributes สำคัญ:
*   `type="text"`: รับข้อมูลอักษรปกติ
*   `type="password"`: รับรหัสผ่าน ปิดบังอักษรจริง
*   `type="email"`: บังคับมีรูปแบบที่ถูกต้องของอีเมล
*   `type="radio"`: เลือกได้หนึ่งเดียวในกลุ่ม ใส่ `name` กลุ่มให้เหมือนกัน
*   `type="checkbox"`: ติ๊กเลือกได้พร้อมกันหลายหัวข้อ

### 3.2 ปุ่มอธิบายข้อมูลผู้ใช้ `<label>` (สำคัญต่อ Accessibility และใช้งานง่าย):
การเชื่อมต่อใช้แอตทริบิวต์ `for` ในแอลเบลลิงก์เข้าคู่กับ `id` ของกล่องอินพุต เมื่อผู้ใช้กดคลิกที่ตัวหนังสือจะโฟกัสเข้ากล่องกรอกข้อมูลทันที
```html
<label for="username">ชื่อผู้ใช้งาน:</label>
<input type="text" id="username" name="username" placeholder="กรอกชื่อเป็นภาษาอังกฤษ" required>
```

### 3.3 แถบเลือก Dropdown และกล่องข้อความบรรยายขนาดใหญ่:
```html
<!-- แถบ Dropdown -->
<label for="province">เลือกจังหวัด:</label>
<select id="province" name="province">
    <option value="">-- เลือกจังหวัดของคุณ --</option>
    <option value="bkk">กรุงเทพมหานคร</option>
    <option value="cm">เชียงใหม่</option>
</select>

<!-- กล่องข้อความบรรยาย -->
<label for="note">บันทึกเพิ่มเติม:</label>
<textarea id="note" name="note" rows="4" cols="50" placeholder="ระบุรายละเอียด..."></textarea>
```

---

## 📝 ใบงานปฏิบัติท้ายบทเรียน (Assignment 4)

**โจทย์:** ให้นักเรียนสร้างหน้าเว็บลงทะเบียนสมาชิกใหม่ในไฟล์ชื่อ `register.html` โดยมีโครงสร้างดังนี้:
1.  ด้านบนสุดแสดงหัวข้อด้วยอักษรตัวใหญ่ว่า **"ฟอร์มสมัครสมาชิกใหม่"** พร้อมมีคำบรรยายเตือนสั้นๆ
2.  สร้างกล่องกลุ่มข้อมูลด้วยการใช้แท็ก `<fieldset>` และมีชื่อกลุ่มกำกับขอบกล่องด้วย `<legend>` เขียนว่า **"ข้อมูลส่วนตัวทั่วไป"**
3.  ด้านในกล่องข้อมูลทั่วไป ประกอบด้วยฟิลด์ป้อนข้อมูลเหล่านี้ (ต้องเชื่อม `label` และมี `id` ทุกฟิลด์):
    *   *ชื่อผู้ใช้:* เป็นแท็ก text บังคับกรอก (`required`) พร้อมมี placeholder ไกด์
    *   *อีเมล:* บังคับต้องเป็นรูปแบบอีเมล
    *   *รหัสผ่าน:* ปิดแสดงตัวอักษร
    *   *เพศ:* ปุ่มวิทยุ (Radio) ให้เลือก เพศชาย, เพศหญิง, หรือ ไม่ระบุ
    *   *ความสนใจหลัก:* ติ๊กกล่องสี่เหลี่ยม (Checkbox) มีตัวเลือก HTML, CSS, JavaScript
    *   *จังหวัด:* เมนู dropdown (Select) ตัวเลือก กรุงเทพฯ, เชียงใหม่, ภูเก็ต
    *   *ข้อความแนะนำตัวสั้นๆ:* เป็น textarea สูง 5 แถว
4.  จัดทำปุ่มกดยืนยันการสมัครและปุ่มล้างข้อมูลโดยใช้แท็ก `<button type="submit">` และ `<button type="reset">`
