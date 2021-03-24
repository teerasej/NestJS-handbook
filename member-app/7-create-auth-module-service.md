
# สร้าง module และ service ในการจัดการ Authentication

## สร้าง module และ service

รันคำสั่ง

```
nest g module auth
nest g service auth
```

## import UserModule เพื่อใช้ใน AuthService

```ts
// src/auth/auth.module.ts
import { UserModule } from './../user/user.module';
import { Module } from '@nestjs/common';
import { AuthService } from './auth.service';

@Module({
    // import UserModule
  imports: [UserModule],
  providers: [AuthService]
})
export class AuthModule {}

```
