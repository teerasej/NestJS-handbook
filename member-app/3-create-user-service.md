

# สร้าง Service สำหรับจัดการข้อมูล User ใน Database 

รันคำสั่งด้านล่างเพื่อสร้าง user service

```
nest g service user --no-spec
```

จะได้ไฟล์ `src/user/user.service.ts`

```ts
// src/user/user.service.ts

import { Injectable } from '@nestjs/common';
import { UserDto } from 'src/model/user-dto';

@Injectable()
export class UserService {

    // เขียนกำหนด method ที่รับข้อมูล user ที่จะสร้างเพิ่มใน Database
    create(user:UserDto) {

    }

}

```