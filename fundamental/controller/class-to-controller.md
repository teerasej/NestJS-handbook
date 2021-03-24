
# การเปลี่ยน Class เป็น Controller

การประกาศใช้งาน Class ในภาษา TypeScript สามารถทำได้ตามโครงสร้างนี้ 

```ts
class UserController {

    sayHi(): string {
        return 'hi'
    }

    search() {

    }

}
```


**แต่ถ้าเกิดเราอยากให้ Class ธรรมดาๆ นี้เป็นตัวแทนของ Route ใน Web API ของเราล่ะ? **

NestJS ได้สร้าง Decorator ต่างๆ มาให้เรากำหนดใช้งานกับ Class ได้

```ts
// กำหนด prefix path ของ controller เป็น /users
@Controller('users')
class UserController {


    // กำหนดการส่ง request มาที่ /users แบบ GET method จะมาเรียกใช้ method ใน class นี้
    @Get()
    sayHi(): string {
        return 'hi'
    }

    // กำหนดการส่ง request มาที่ /users แบบ POST method จะมาเรียกใช้ method ใน class นี้
    @Post('/search')
    search(): string {
        return 'search'
    }
}
```