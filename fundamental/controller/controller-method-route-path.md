
# กำหนด Request Decorator สำหรับ method ใน Controller

เราสามารถกำหนด การทำงานของ Request กับ method ใน Class ได้หลากหลายรูปแบบ เพราะมี Decorator ในส่วนนี้เยอะมาก เช่น 


## Request Method

```ts
import { Controller, Get, Post } from '@nestjs/common';

@Controller()
export class AppController {

  // สามารถกำหนด รับ request แบบ GET method 
  // request ที่จะเรียกใช้ method นี้ได้ ต้องส่งมาที่ / แบบ GET เท่านั้น
  @Get()
  getHello(): string {
    return 'hello';
  }

  // สามารถกำหนด แบบ POST method 
  // request ที่จะเรียกใช้ method นี้ได้ ต้องส่งมาที่ / แบบ POST เท่านั้น
  @Post()
  getHelloPost(): string {
    return 'hello POST';
  }


}
```

## กำหนด Route path ให้ Request

เราสามารถกำหนด Route Path ให้ Request decorator ได้ด้วย 

- Route path จะอ้างอิงกับของ Controller อีกที เช่น  
  - ถ้ากำหนด `@Controller('product')`
  - และ `@Get('latest')` 
  - route path ที่สมบูรณ์จะเป็น **/product/latest**


```ts

import { Controller, Get } from '@nestjs/common';

@Controller()
export class AppController {

  //...

  // request ที่จะเรียกใช้ method นี้ได้ ต้องส่งมาที่ /morning แบบ GET เท่านั้น
  @Get('/morning')
  getGoodMorning(): string {
      return 'Good Morning'
  }

  // request ที่จะเรียกใช้ method นี้ได้ ต้องส่งมาที่ /night แบบ GET เท่านั้น
  @Get('/night')
  getGoodNight(): string {
      return 'Good Night'
  }
}
```

## URL Path Parameter 

หากเราต้องการ เราสามารถดึง Parameter ผ่าน URL Path ได้ด้วย `@Param()` decorator

```ts
import { Controller, Get, Param } from '@nestjs/common';

@Controller()
export class AppController {

    //...

    // request ที่จะเรียกใช้ method นี้ได้ ต้องส่งมาที่ /product/{:id} แบบ GET เท่านั้น 
    // ซึ่งค่าที่อยู่ตรงตำแหน่งใน Route path :id จะถูกแปลงเป็น parameter ชื่อ id ตามท่ี่กำหนดไว้ใน @Param decorator
    @Get('products/:id')
    getProduct(@Param('id') id) {
        return id;
    }


    @Get('movies/:year/:month') 
    getMovies(@Param('year') year, @Param('month') month) {
        return {
            movieYear: year,
            movieMonth: month
        }
    }
    

}
```

