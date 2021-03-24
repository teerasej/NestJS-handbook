
# สร้างกลไกการ Authentication ด้วย JWT

## 1. สร้าง JWT strategy 

รอบนี้เราจะสร้างไฟล์ strategy แบบที่ทำงานกับ JWT ซึ่งก็จะมีการสร้าง Guard มาเรียกใช้ Strategy นี้อีกทีหนึ่ง

```ts
// src/auth/jwt.strategy.ts

import { ExtractJwt, Strategy } from 'passport-jwt';
import { PassportStrategy } from '@nestjs/passport';
import { Injectable } from '@nestjs/common';
import { jwtConstants } from './constants';

@Injectable()
// สังเกตว่าเป็น Class ที่ extend PassportStrategy แบบเดียวกับ local ที่เราทำก่อนหน้า
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor() {
    super({
    // `jwtFromRequest` กำหนดให้หา Bearer Token จาก header ของ Request
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
    // `ignoreExpiration` ให้ตรวจว่า token หมดอายุหรือไม่
      ignoreExpiration: false,
    // `secretOrKey` เทียบค่า key 
      secretOrKey: jwtConstants.secret,
    });
  }

    // `validate()` method จะได้รับ payload ที่ถอดข้อมูลกลับมาใช้งาน
  async validate(payload: any) {
    return { email: payload.email };
  }
}
```

## 2. สร้าง AuthGuard แบบ JWT

```ts
// src/auth/jwt-auth.guard.ts
import { Injectable } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {}

```

## 3. กำหนดใช้ JWTStrategy เป็น provider ใน Auth Module

```ts
// src/auth/auth.module.ts

import { UserModule } from './../user/user.module';
import { Module } from '@nestjs/common';
import { AuthService } from './auth.service';
import { PassportModule } from '@nestjs/passport';
import { LocalStrategy } from './local.strategy';
import { JwtModule } from '@nestjs/jwt';
import { jwtConstants } from './constants';
import { JwtStrategy } from './jwt.strategy';

@Module({
  imports: [
    UserModule, 
    PassportModule,
    JwtModule.register({
      secret: jwtConstants.secret,
      signOptions: { expiresIn: '5d' },
    }),
  ],
  // กำหนดใช้ JwtStrategy
  providers: [AuthService, LocalStrategy, JwtStrategy],
  exports: [AuthService, JwtModule],
})
export class AuthModule {}

```

## 4. สร้าง route /users/profile ที่ปกป้องด้วย AuthGuard 

- ค่าที่ return ได้จาก AuthGuard จะอยู่ใน Request ในชื่อ property `.user` 

```ts
// src/app.controller.ts
import { Body, Controller, Get, Post, Request, UseGuards } from '@nestjs/common';
import { AppService } from './app.service';
import { AuthService } from './auth/auth.service';
import { JwtAuthGuard } from './auth/jwt-auth.guard';
import { LocalAuthGuard } from './auth/local-auth.guard';
import { UserDto } from './model/user-dto';
import { UserService } from './user/user.service';

@Controller()
export class AppController {
  constructor(private readonly userService: UserService
    , private authService:AuthService) {}

  // @Get()
  // getHello(): string {
  //   return this.appService.getHello();
  // }

  @Post('signup')
  signup(@Body() user:UserDto) {
    return this.userService.create(user);
  }

  @UseGuards(LocalAuthGuard)
  @Post('login')
  async login(@Request() req) {
    return this.authService.login(req.user);;
  }

    // ใช้ JwtAuthGuard เพื่อตรวจเช็ค Request ที่ส่งเข้ามาที่ route path นี้ว่ามี token ที่เหมาะสมไหม
  @UseGuards(JwtAuthGuard)
  @Get('users/profile')
  getProfile(@Request() req) {
      // ค่าที่ return ได้จาก AuthGuard จะอยู่ใน Request ในชื่อ property `.user`
    return req.user;
  }
}

```