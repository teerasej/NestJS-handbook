
# validate ข้อมูล user ผ่าน UserService 

- ถ้าพบให้คืนข้อมูล user ออกไปจาก method 
- ถ้าไม่พบให้คืนค่า null

```ts
// src/auth/auth.service.ts

import { Injectable } from '@nestjs/common';
import { UserService } from 'src/user/user.service';

@Injectable()
export class AuthService {
    constructor(private usersService: UserService) {}

    async validateUser(email: string, pass: string): Promise<any> {

        // ค้นหา User ด้วย email ผ่าน User service
        const user = await this.usersService.findOne(email);
        
        // เทียบข้อมูล User 
        if (user && user.password === pass) {

          // ถ้าพบ User ที่ตรงกับข้อมูลใน Databsae และรหัสผ่านตรงกัน ให้คืนค่าเป็น user นั้น
          return user
        }
        
        // ถ้าไม่พบ User ที่ตรงกับข้อมูลใน Databsae ให้คืนค่า null
        return null;
      }
}

```