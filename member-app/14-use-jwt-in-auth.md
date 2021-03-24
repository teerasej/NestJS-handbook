
# สร้างกลไกการสร้าง token ด้วย JWT module และ JWT service

## 1. ใช้ JWTModule กำหนดข้อมูลสำหรับใช้จัดการ token


```ts
// src/auth/auth.module.ts

import { UserModule } from './../user/user.module';
import { Module } from '@nestjs/common';
import { AuthService } from './auth.service';
import { PassportModule } from '@nestjs/passport';
import { LocalStrategy } from './local.strategy';
import { JwtModule } from '@nestjs/jwt';
import { jwtConstants } from './constants';

@Module({
  imports: [
    UserModule, 
    PassportModule,

    // เรียกใช้ JWT Module กำหนดเงื่อนไขในการทำงานของของ Token
    // สามารถเรียกใช้ JWTService จาก module นี้ได้
    JwtModule.register({
        // กำหนด secret key
      secret: jwtConstants.secret,
      // กำหนดอายุของ token
      signOptions: { expiresIn: '5d' },
    }),
  ],
  providers: [AuthService, LocalStrategy],

  // Export JWT Module 
  exports: [AuthService, JwtModule],
})
export class AuthModule {}

```

## 2. ใช้ JWT Service สร้าง token จาก payload payload เป็น string หรือ object หนึ่งที่นำไปรวมกับ secret key และอื่นๆ

```ts
// src/auth/auth.service.ts
import { Injectable } from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';
import { UserService } from 'src/user/user.service';

@Injectable()
export class AuthService {
    constructor(
        private usersService: UserService,
        // เรียกใช้ JWT Service ในชื่อ jwtService
        private jwtService: JwtService) {}

    async validateUser(email: string, pass: string): Promise<any> {
        const user = await this.usersService.findOne(email);
        if (user && user.password === pass) {
          return user
        }
        return null;
      }

        // สร้าง Method login ที่รับข้อมูล user มาใช้ประกอบการสร้าง token
      async login(user: any) {

          // กำหนดข้อมูลลงไปใน payload เพื่อใช้ในการสร้าง token
        const payload = { email: user.email };

        // สร้าง token ด้วย jwt service และ return ค่าเป็น object ที่มี access_token นำไปใช้งานต่อ
        return {
          access_token: this.jwtService.sign(payload),
        };
      }
}

```

## 3. สร้าง access token จากข้อมูล user 

- ข้อมูลที่ได้จาก LocalAuthGuard จะถูกส่งออกมาในชื่อของ `.user` ใน request 
- เราส่งข้อมูลต่อให้ AuthService เพื่อเอาข้อมูลไปใช้ในการสร้าง token

```ts
// src/app.controller.ts

import { Body, Controller, Get, Post, Request, UseGuards } from '@nestjs/common';
import { AppService } from './app.service';
import { AuthService } from './auth/auth.service';
import { LocalAuthGuard } from './auth/local-auth.guard';
import { UserDto } from './model/user-dto';
import { UserService } from './user/user.service';

@Controller()
export class AppController {
  constructor(
      private readonly userService: UserService,
      // เรียกใช้งาน auth service
      private authService:AuthService) {}


  @Post('signup')
  signup(@Body() user:UserDto) {
    return this.userService.create(user);
  }

  @UseGuards(LocalAuthGuard)
  @Post('login')
  async login(@Request() req) {
    // ข้อมูลที่ได้จาก LocalAuthGuard จะถูกส่งออกมาในชื่อของ `.user` ใน request 
    // เราส่งข้อมูลต่อให้ AuthService เพื่อเอาข้อมูลไปใช้ในการสร้าง token
    return this.authService.login(req.user);
  }
}

```