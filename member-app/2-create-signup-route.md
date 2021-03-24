
# สร้าง Route สำหรับรับข้อมูลลงทะเบียน

## 1. สร้าง Data Transfer Object รับข้อมูลจาก request

เนื่องจากเรากำหนดในที่นี้ว่าข้อมูลที่ส่งมาจาก Client จะมีหน้าตาแบบด้านล่าง 

```json
{
    "email":"training@nextflow.in.th",
    "password":"1234"
}
```

เราจะสร้าง class ที่มีโครงสร้างแบบเดียวกัน โดยใช้คำสั่ง 

```
nest g class model/userDto --no-spec
```

แล้วเขียนกำหนด property ชื่อ `email` และ `password`

```ts
// src/model/user-dto.ts

export class UserDto {

    email:string
    password:string

}

```

## 2. สร้าง signup route path ใช้ Class DTO รับข้อมูล JSON จาก request 

ในตอนนี้ให้ return ข้อมูล signup กลับออกไปก่อน

```ts
// src/app.controller.ts
import { Body, Controller, Get, Post } from '@nestjs/common';
import { AppService } from './app.service';
import { UserDto } from './model/user-dto';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }

    // กำหนดให้ method นี้รัน POST request ที่ส่งมาที่ route path /signup
  @Post('signup')
  signup(@Body() user:UserDto) {
      // คืนค่า user ที่ได้กลับไปให้ client
    return user;
  }
}

```

