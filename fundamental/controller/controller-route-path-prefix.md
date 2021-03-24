
# การกำหนด Route Path Prefix ใน Controller

เราสามารถกำหนด Route path prefix ได้ในส่วนของ Controller Decorator ได้ดังนี้ 

```ts
// กำหนด prefix path ของ controller เป็น /
// request ที่จะมาเรียกใช้ controller ตัวนี้ จะต้องถูกส่งมาที่ http://localhost:3000/
@Controller()
class AppController {

}


// กำหนด prefix path ของ controller เป็น /users
// request ที่จะมาเรียกใช้ controller ตัวนี้ จะต้องถูกส่งมาที่ http://localhost:3000/users
@Controller('users')
class UserController {

}


// กำหนด prefix path ของ controller เป็น /users/admin
// request ที่จะมาเรียกใช้ controller ตัวนี้ จะต้องถูกส่งมาที่ http://localhost:3000/users/admin
@Controller('users/admin')
class AdminController {

}
```