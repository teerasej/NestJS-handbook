

# การรับข้อมูลแบบ JSON จาก Request

ในปัจจุบัน การส่งข้อมูลจาก Client อาจจะมาในรูปแบบ JSON ซึ่งเราสามารถแปลงมาใช้ในรูปแบบของ Data Type เป็น Class ที่เราต้องการได้ 

## 1. สร้าง Class Data Transfer Object (DTO) 

เราจำเป็นต้องสร้าง Class ที่มีโครงสร้างข้อมูลแบบเดียวกับข้อมูล JSON ที่จะได้รับ เช่น ถ้าข้อมูล json มาเป็นแบบนี้

```json
{
    "email":"training@nextflow.in.th",
    "password":"1234"
}
```

เราต้องสร้าง Class แบบนี้ (ชื่อ Class ไม่จำเป็นต้องมีคำว่า DTO)

```ts
export class UserDTO {
    email:string
    password:string
}
```

## 2. ใช้ @Body() decorator ในการแปลงข้อมูลของ Request ใน Method

```ts
@Controller()
export class AppController {

    //..

    // @Body() จะดึงค่าที่อยู่ใน Body ของ Request มาแปลงใส่ instance ของ Class ให้
    @Post('signup')
    async signUp(@Body() user: UserDTO) {

        console.log(user);
        return user;
    }

}
```

## 3. การ Validate ข้อมูลที่มากับ Body ของ Request 

์มี package [class-validator](https://github.com/typestack/class-validator) ที่มี decorator ที่สามารถนำมาใช้งาน validate ข้อมูลที่กำหนดให้กับ property ของ Class ได้ 

และเราสามารถนำมาประยุกต์ใช้กับ DTO ได้ด้วย

```ts
import { IsEmail, IsNotEmpty } from 'class-validator'

export class UserDTO {

    @IsEmail()
    email:string

    @IsNotEmpty()
    password:string
}

```