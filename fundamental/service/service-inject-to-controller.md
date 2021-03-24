
# การเรียกใช้ Service ใน Controller

## ขั้นตอนการ Injection

การเรียกใช้งาน service จาก provider จะเกิดขั้นตอนดังนี้

1. จะมีการสร้าง instance หรือ object ของ provider ที่ถูกเรียกใช้ 
2. instance (หรือ object) จะถูกจัดการดูแลโดยระบบของ module
3. reference ของ instance หรือ object จะถูกส่งเข้ามาผ่าน constructor ของ class ที่มีการเรียกใช้

> class ที่เรียกใช้ provider จะเป็น controller หรือ provider ตัวอื่นก็ได้


```ts
@Controller()
export class AppController {
  constructor(
    // กำหนดเรียกใช้ provider ชื่อ MyService ในชื่อ property: myService
    private readonly myService: MyService) {

    }

```

- **การทำแบบนี้จะเป็นไปไม่ได้เลย ถ้าไม่มีการกำหนดชื่อ Service ไว้ในส่วน provider ของ module ที่เหมาะสม**

## การเรียกใช้ 

หลังจากเราได้ provider มาในลักษณะ 