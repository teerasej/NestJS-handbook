

# บันทึกผู้ใช้ใหม่ลงในฐานข้อมูล

## 1. ใช้ user model สร้าง document ใหม่ใน database 

- การสั่งใช้ method `save()` จะเป็นการยืนยันการสร้างข้อมูลใหม่ 
- ทำการ return document ที่สร้างขึ้น


```ts
// src/user/user.service.ts

import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { UserDto } from 'src/model/user-dto';
import { User, UserDocument } from './user.schema';

@Injectable()
export class UserService {
    constructor(@InjectModel(User.name) private userModel: Model<UserDocument>) {}

    // กำหนดให้ method นี้ทำงานแบบ async และคืนค่าเป็น data type User
    async create(user:UserDto): Promise<User> {
        // สร้าง user model จากข้อมูล
        const newUser = new this.userModel(user);

        // สั่ง save() เพื่อยืนยันการทำงาน
        return newUser.save()
    }

}

```

## 2. บันทึก user ใหม่ลงฐานข้อมูลด้วย UserService 

- ทำการ import `UserService` เข้ามาใน `AppController` ผ่าน `contructor()` method
- เรียกใช้ method `create()` 
 
การทำแบบนี้ได้ เ**พราะมีการ import User Module เข้ามาใน App Module** (สามารถสังเกตได้ในไฟล์ `app.module.ts` ในส่วน `providers:`)

```ts
// src/app.controller.ts

import { Body, Controller, Get, Post } from '@nestjs/common';
import { AppService } from './app.service';
import { UserDto } from './model/user-dto';
import { UserService } from './user/user.service';

@Controller()
export class AppController {
    // เรียกใช้ User Service ผ่าน constructor ในชื่อของ userService สำหรับ AppController นี้
  constructor(private readonly userService: UserService) {}

  // @Get()
  // getHello(): string {
  //   return this.appService.getHello();
  // }

  @Post('signup')
  signup(@Body() user:UserDto) {
    // เรียกใช้คำสั่ง create และส่งค่าผลลัพธ์ที่ได้กลับไปให้ client
    return this.userService.create(user);
  }
}

```