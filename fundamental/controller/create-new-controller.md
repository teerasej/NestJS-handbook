
# การสร้าง Controller ใหม่ ใช้งานในโปรเจค

เราสามารถสร้างโครงของ controller ใหม่ได้ ด้วยคำสั่ง `nest`

```
nest g controller ชื่อ 
```

- ถ้าไม่ต้องการให้สร้าง ให้เพิ่ม option `--no-spec` เข้าไปในคำสั่ง
- ถ้าต้องการสร้างไว้ใน folder ที่ต้องการ สามารถใส่แบบนี้ได้ `{ชื่อ folder}/{ชื่อ controller}` เช่น `controllers/user`

การทำแบบนี้ nest cli จะไปเพิ่มชื่อ controller ในไฟล์ module ด้วย

```ts
@Module({
  controllers: [
      AppController, 
      MyController // Controller ที่สร้างจากคำสั่ง nest g จะถูกเพิ่มเข้า module มาตรฐานอัตโนมัติ
    ],
  providers: [AppService],
})
```