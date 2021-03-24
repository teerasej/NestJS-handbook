
# สร้าง Local strategy สำหรับการ Authen ด้วย username และ password


## 1. เรียกใช้ passport module และ LocalStrategy ใน Auth Module 

```ts
// src/auth/auth.module.ts

import { UserModule } from './../user/user.module';
import { Module } from '@nestjs/common';
import { AuthService } from './auth.service';
import { PassportModule } from '@nestjs/passport';
import { LocalStrategy } from './local.strategy';

@Module({
    // import PassportModule
  imports: [UserModule, PassportModule],
  // เรียกใช้ LocalStrategy
  providers: [AuthService, LocalStrategy]
})
export class AuthModule {}

```

## 2. สร้าง strategy แบบ local สำหรับใช้การ login 

- class ที่สืบทอด `PassportStrategy`  จะมี method `validate()` ซึ่งจะทำงานเมื่อถูกเรียกใช้ ในที่นี้ Guard ใน NestJS จะเป็นคนเรียกใช้ 
- เราจึงเอาค่า email, password ใส่ auth service

```ts
// src/auth/local.strategy.ts

import { Strategy } from 'passport-local';
import { PassportStrategy } from '@nestjs/passport';
import { Injectable, UnauthorizedException } from '@nestjs/common';
import { AuthService } from './auth.service';

@Injectable()
export class LocalStrategy extends PassportStrategy(Strategy) {

    // นำเข้า Auth service มาใช้งานใน class ในชื่อ authService
  constructor(private authService: AuthService) {
    super();
  }

    // validate() จะได้รับค่า 2 ค่าที่ได้จาก request มาใช้งานเป็น parameter
    // ค่าแรกจะได้จากค่า username ที่ส่งมากับ request
    // ค่าที่ 2 จะได้จากค่า password ที่ส่งมากับ request
  async validate(email: string, password: string): Promise<any> {

      // Validate ผ่าน Auth service
    const user = await this.authService.validateUser(email, password);

    // ถ้าไม่ได้ข้อมูล user ออกมา ให้ response กลับไปหา client ว่า Unauthorized (401)
    if (!user) {
      throw new UnauthorizedException();
    }

    // ถ้าได้ข้อมูลให้ส่งค่าที่ได้ ออกไปกับ request ในชื่อ property .user
    return user;
  }
}
```