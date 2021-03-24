

# สร้าง route /login และปกป้องด้วย LocalAuthGuard 

- ซึ่งถ้ามี request ส่งมาที่ route นี้ จะกระตุ้นให้ LocalStrategy ทำงาน และเกิดการ validate 
- ถ้า validate ไม่ผ่าน จะโยน UnauthorizedException 
- ถ้าผ่าน จะนำข้อมูลที่ return ออกมา กำหนดเป็น .user property ใน Request เข้า method ใน controller ปกติ

```ts
// src/app.controller.ts

import { Body, Controller, Get, Post, Request, UseGuards } from '@nestjs/common';
import { AppService } from './app.service';
import { LocalAuthGuard } from './auth/local-auth.guard';
import { UserDto } from './model/user-dto';
import { UserService } from './user/user.service';

@Controller()
export class AppController {
  constructor(private readonly userService: UserService) {}


  @Post('signup')
  signup(@Body() user:UserDto) {
    return this.userService.create(user);
  }

    // ใช้ Guard ที่สร้างไว้ เพื่อส่ง request ให้ strategy ที่เหมาะสม ในที่นี้คือ local
  @UseGuards(LocalAuthGuard)
  // สร้าง Route path /login รับเป็น POST method
  @Post('login')
  async login(@Request() req) {
      // ส่งค่าที่ได้จากการ validate กลับไปให้ client
    return req.user;
  }
}

```