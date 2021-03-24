
# เรียกใช้ Mongoose module และตั้งค่าการใช้งาน

ส่วนนี้ต้องการฐานข้อมูล MongoDB ที่รันทำงานอยู่ในเครื่องคอมเดียวกัน สามารถดูวิธีติดตั้ง และ[รันใช้งานแบบ Docker ได้จากที่นี่](https://github.com/teerasej/docker-handbook/blob/master/simple-mongo/readme.md) 


```ts
// src/app.module.ts

import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { UserService } from './user/user.service';

@Module({
    // import Mongoose module และกำหนด connection string เพื่อเชื่อมต่อไปที่ MongoDB server ที่รันใช้งานในเครื่อง
  imports: [MongooseModule.forRoot('mongodb://localhost/nest')],
  controllers: [AppController],
  providers: [AppService, UserService],
})
export class AppModule {}

```
