
# Docker Deployment

## ใช้งาน `.dockerignore` เพื่อให้ docker ละเว้นการทำงานกับไฟล์ที่ไม่ต้องการ 

ในการ copy ไฟล์จากโปรเจค ไปไว้ใน Image เราสามารถกำหนดรายการของ directory หรือไฟล์ที่ไม่ต้องการให้ docker ทำงานด้วย ผ่านไฟล์ `.dockerignore`

เช่น directory ชื่อ `node_modules` 

```
# .dockerignore
node_modules
```

## การสร้าง Image สำหรับ Web API 

เราสามารถสร้างไฟล์ `Dockerfile` สำหรับเขียนคำสั่งเตรียมการสร้าง Image ขึ้นมาใช้งาน, จัดเก็บ, หรือเผยแพร่ ได้

คำสั่งที่ใช้ใน Dockerfile สามารถ[ดูเพิ่มเติมได้ที่นี่](https://docs.docker.com/engine/reference/builder/#usage)

```dockerfile
# Dockerfile

# ใช้ image ของ node เวอร์ชั่น lts ล่าสุด
FROM node:lts-alpine3.10

# set ตำแหน่งอ้างอิงไปที่ /usr/src/app ที่อยู่ใน Image
WORKDIR /usr/src/app

# copy ไฟล์ package*.json จากที่อยู่อ้างอิง ไปไว้ในโฟลเดอร์อ้างอิง
# COPY [original file] [Image directory]
COPY package*.json .

# รันคำสั่งจาก terminal ใน Image ในที่นี้คือ npm install เพื่อติดตั้ง module ที่จำเป็นทั้งหมด
RUN npm install 

# คัดลอกไฟล์ทั้งหมดในโฟลเดอร์ที่ Dockerfile อยู่ ไปยัง โฟลเดอร์เป้าหมายบน Image (อ้างอิงจาก WORKDIR ในที่นี่คือ /usr/src/app)
# ไฟล์ และ directory ที่ระบุใน .dockerignore จะถูกละเว้นจากคำสั่งนี้
COPY . . 

# กำหนดให้ Image เปิด PORT 3000 ให้ภายนอกเข้าถึงได้
EXPOSE 3000

# รันคำสั่ง npm start ตอน container เริ่มการทำงาน
CMD ["npm","start"]
```

### สร้าง Image จาก Dockerfile

จากนั้นเราสามารถสร้าง Image ได้ด้วยคำสั่ง ([ดู option ของคำสั่งเพิ่มเติมได้ที่นี่](https://docs.docker.com/engine/reference/commandline/image_build/))

```
docker image build . --tag web-api
```

- `--tag`: กำหนดชื่อของ Image และสามารถกำหนด tag ได้ด้วยเช่น `--tag super-api`, `--tag super-api:dev`, `--tag super-api:prod`


### รัน Container ใช้งาน จาก Image 

เราสามารถรันคำสั่งด้านล่าง เพื่อรัน Container ขึ้นมาจาก Image และทำการ map port

```
docker run -p 80:3000 web-api
```

- `-p EXTERNAL_PORT:INTERNAL_PORT`: ทำการจับคู่ port ภายนอก กับหมายเลข port ภายใน container
- ในที่นี้คือการ map port 80 ของ Container กับ Port 3000 ภายใน Container เราสามารถ **แน่นอนว่าตอนสร้าง Image ต้องมีการสั่ง เปิด port ดังกล่าวด้วยคำสั่ง `EXPOSE`**


## การเชื่อมต่อกับ MongoDB ใน Docker Container 

สำหรับการเชื่อมต่อกับ mongo container ในการรันระบบขึ้นมาด้วย Docker Compose เราสามารถอ้างอิงกับชื่อ Mongo Container ได้

```yaml
# docker-compose.yaml

version: "2"
services: 
  web:
    build: .
    ports:
      - "80:3000"
    # link ไปที่ mongo container
    links:
      - mongo

  # Container ชื่อ mongo
  mongo:
      image: mongo
      ports:
        - "27017:27017"
```

ดังนั้นใน connection string เราสามารถกำหนดเป็น

```ts
// src/app.module.ts

@Module({
  imports: [
      // ปกติจะเป็น mongodb://{server}:{port}/{database name} เช่น
      // mongodb://localhost:27017/shop 
      // ใช่ไหมล่ะ?

      // แทนที่ส่วนของ server กับ port ด้วยชื่อของ container จะเป็น
      // mongodb://mongo/shop
      MongooseModule.forRoot('mongodb://mongo/shop'),
      //...
    ],
  //...
})
```

สามารถรันคำสั่ง Docker compose สำหรับการสร้าง Image ใหม่ทุกครั้งด้วยคำสั่งด้านล่าง

```
docker-compose up --build
```

ถ้าไม่ต้องการให้สร้าง Image ใหม่ รันแค่คำสั่งด้านล่างพอ

```
docker-compose up
```