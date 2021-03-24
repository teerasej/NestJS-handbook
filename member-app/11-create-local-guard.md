

#  สร้าง Guard ที่ใช้ local strategy ของ passport 

- Guard เป็นส่วนที่จะรับ Request มาพิจารณาว่าจะให้ผ่านเข้า method หรือไม่
- โดยการ extend class `AuthGuard('local')` ซึ่ง auth guard จะใช้ class ที่ extend LocalStrategy ที่เราสร้างไว้

```ts
// src/auth/local-auth.guard.ts

import { Injectable } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class LocalAuthGuard extends AuthGuard('local') {}
```