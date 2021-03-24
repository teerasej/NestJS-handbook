
# ทดสอบรันโปรเจคแบบธรรมดา และแบบ auto-reload

เราสามารถรันคำสั่งผ่านโปรแกรม Terminal (MacOS) หรือ Command Prompt (Windows) เพื่อเริ่มต้นการทำงานของ NestJS ได้

## รันโปรเจคแบบธรรมดา

```
npm start
```

หรือ ถ้าใครใช้ yarn

```
yarn start
```

## รันโปรเจคแบบ auto-reload

การรันในโหมด development จะทำให้การแก้ไขไฟล์ใดๆ ในโปรเจค จะเป็นการรีโหลดตัวโค้ด และ server ใหม่ ทำให้ประหยัดเวลา

สามารถทำได้ด้วยคำสั่ง

```
npm run start:dev
```

หรือ ถ้าใครใช้ yarn

```
yarn run start:dev
```
