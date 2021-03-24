
# การสร้าง Service

เราสามารถสร้าง Service โดยใช้คำสั่ง

```
nest g service ชื่อ
```

> แน่นอนว่าเราสามารถสั่งสร้าง Service ไว้ภายในโฟลเดอร์ย่อยได้ด้วย เหมือนกับ [Controller](../controller/create-new-controller.md)

การทำแบบนี้ nest cli จะไปเพิ่มชื่อ service ในส่วนที่ชื่อว่า `providers` ของไฟล์ module ด้วย

```ts
@Module({
  controllers: [
      AppController, 
      //...
    ],
  providers: [
      AppService,
      MyService // Services ที่สร้างจากคำสั่ง nest g จะถูกเพิ่มเข้าส่วน providers ของ module มาตรฐานอัตโนมัติ
    ],
})
```