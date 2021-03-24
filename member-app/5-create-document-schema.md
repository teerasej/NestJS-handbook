

# สร้าง schema ของข้อมูล User สำหรับใช้เป็นตัวแทนใน Database 

## 1. สร้างไฟล์ Schema สำหรับข้อมูล User

ให้สร้างไฟล์ `src/user/user.schema.ts` 
และกำหนด prop ของ document เป็น `email` และ `password`

```ts
// src/user/user.schema.ts

import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';

// กำหนด Data type ของ Schema
export type UserDocument = User & Document;

@Schema()
export class User {
    
    // กำหนด field ของ Document ใน Collection ด้วย @Prop() decorator
  @Prop()
  email: string;

  @Prop()
  password: string;

}

// นำ Class ที่ได้มาแปลงเป็น Schema เพื่อเอาไปใช้งานใน Mongoose
export const UserSchema = SchemaFactory.createForClass(User);
```

## 2. สร้าง module สำหรับ user โดยเฉพาะ 

- มีการกำหนด UserDocument Type และ UserSchema
- Export UserService เพื่อให้ module อื่นสามารถเรียกใช้ได้ 

```ts
import { MongooseModule } from '@nestjs/mongoose';
import { Module } from '@nestjs/common';
import { User, UserSchema } from './user.schema';
import { UserService } from './user.service';

@Module({

    // ใช้ Mongoose Module สร้าง schema และชื่อของ data type ใช้งาน
    imports: [MongooseModule.forFeature([{ name: User.name, schema: UserSchema }])],
    providers: [UserService],

    // export UserService เพื่อทำให้ module ที่นำ User module นี้ไปใช้ สามารถเข้าถึงและเรียกใช้งาน UserService ได้
    exports: [UserService]
  })
export class UserModule {}

```

## 3. Inject model ของ User ที่สร้างไว้ เป็นตัวแทนของ Collection User ใน Database

```ts
// src/user/user.service.ts

import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { UserDto } from 'src/model/user-dto';
import { User, UserDocument } from './user.schema';

@Injectable()
export class UserService {
    // เราสามารถเรียกใช้ this.userModel ในกระบวนการจัดการข้อมูลที่เกี่ยวกับ User ใน Database ได้
    constructor(@InjectModel(User.name) private userModel: Model<UserDocument>) {}

    create(user:UserDto) {

    }

}

```