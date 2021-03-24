
# Member App

- มีระบบ Sign up
- สามารถ login และสร้าง token ส่งกลับมาให้ client ได้
- การขอข้อมูลผู้ใช้ จะต้องมีการส่ง token มาที่ Route URL ด้วย


[รันใช้งานแบบ Docker ได้จากที่นี่](https://github.com/teerasej/docker-handbook/blob/master/simple-mongo/readme.md) 


## สร้าง Route และ Service

1. [สร้างโปรเจค และติดตั้ง package](1-setup-project.md) 
2. [สร้าง Route สำหรับรับข้อมูลลงทะเบียน](2-create-signup-route.md)
3. [สร้าง Service สำหรับจัดการข้อมูล User ใน Database](3-create-user-service.md)

## ทำงานกับฐานข้อมูล MongoDB

4. [เรียกใช้ Mongoose module และตั้งค่าการใช้งาน](4-mongoose-setup.md)
5. [สร้าง schema ของข้อมูล User สำหรับใช้เป็นตัวแทนใน Database](5-create-document-schema.md)
6. [บันทึกผู้ใช้ใหม่ลงในฐานข้อมูล](6-create-new-user.md)

## ระบบ Authentication แบบใช้ Username/Password

7. [สร้าง module และ service ในการจัดการ Authentication](7-create-auth-module-service.md)
8. [สร้างกลไกหา User ใน Database ด้วย email](8-find-user-with-email.md)
9. [validate ข้อมูล user ผ่าน UserService](9-validate-userdata.md)
10. [สร้าง Local strategy สำหรับการ Authen ด้วย username และ password](10-create-local-strategy-and-guard.md)
11. [สร้าง Guard ที่ใช้ local strategy ของ passport](11-create-local-guard.md)
12. [สร้าง route /login และปกป้องด้วย LocalAuthGuard](12-route-login-with-guard.md)

การทดสอบส่ง request ไปที่ route ที่ปกป้องด้วย Guard ที่ใช้ local authen ควรมี property เป็น username และ password

```json
{
    "username": "pon@gmail.com",
    "password": "1234"
}
```

## ระบบ Authentication แบบใช้ JWT

13. [กำหนด secret key สำหรับสร้าง token](13-create-secret-key.md)
14. [สร้างกลไกการสร้าง token ด้วย JWT module และ JWT service](14-use-jwt-in-auth.md)
15. [สร้างกลไกการ Authentication ด้วย JWT](15-authen-with-jwt.md)

