<div align="center">

# 🎬 Dramabox API

### REST API สำหรับเข้าถึงเนื้อหา Dramabox

[![Node.js](https://img.shields.io/badge/Node.js-18+-339933?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express-4.x-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com/)
[![License](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)
[![Version](https://img.shields.io/badge/Version-1.3.0-green?style=for-the-badge)]()

[🚀 Demo](https://dramabox-rest-api-node-rho.vercel.app/) • [📖 เอกสาร](#-endpoints) • [🐛 แจ้งปัญหา](https://github.com/hndko/dramabox-rest-api-node/issues)

</div>

---

## ✨ คุณสมบัติ

| คุณสมบัติ              | รายละเอียด                          |
| ---------------------- | ----------------------------------- |
| 🔍 **ค้นหา**           | ค้นหาซีรีส์ตามคำค้น                  |
| 📺 **สตรีมมิ่ง**        | รับ URL สตรีมมิ่ง (m3u8/mp4)        |
| 📋 **รายการตอน**       | รายการตอนทั้งหมด (รองรับ 500+ ตอน)  |
| 🏷️ **หมวดหมู่**        | สำรวจตามหมวดหมู่                    |
| ⭐ **แนะนำ**           | ซีรีส์ที่แนะนำ                       |
| 👑 **เนื้อหา VIP**     | เข้าถึงเนื้อหา VIP/Theater          |
| 🌐 **หลายภาษา**        | รองรับ ไทย/อินโด/อังกฤษ (th/in/en) |

## 🛡️ Production Ready

| Best Practice          | สถานะ           |
| ---------------------- | --------------- |
| ⚡ Rate Limiting       | ✅ 100 req/min  |
| 🗜️ Gzip Compression    | ✅ ~70% smaller |
| 🔒 Security Headers    | ✅ Helmet       |
| 🔄 Auto Retry          | ✅ 3x + backoff |
| 💾 Response Caching    | ✅ 5-60 min TTL |
| 📊 Health Check        | ✅ /health      |
| 🎯 Input Validation    | ✅ Sanitized    |
| 🚦 Graceful Shutdown   | ✅ SIGTERM      |

---

## 🚀 เริ่มต้นใช้งาน

### ข้อกำหนดเบื้องต้น

- Node.js 18+
- npm หรือ yarn

### การติดตั้ง

```bash
# Clone repository
git clone https://github.com/Popetza38/api.git
cd dramabox-rest-api-node

# ติดตั้ง dependencies
npm install

# Build CSS (ไม่บังคับ)
npm run build:css

# เริ่มเซิร์ฟเวอร์สำหรับพัฒนา
npm run dev
```

### Environment Variables (ไม่บังคับ)

```env
PORT=3000
NODE_ENV=development
DEFAULT_LANG=th
```

---

## 📖 Endpoints

### Base URL

```
Local: http://localhost:3000
Production: https://dramabox-rest-api-node-rho.vercel.app
```

### 🔍 ค้นหาซีรีส์

```http
GET /api/search?keyword={keyword}&page={page}&size={size}&lang={lang}
```

| Parameter | Type   | Required | Default | Description     |
| --------- | ------ | -------- | ------- | --------------- |
| keyword   | string | ✅       | -       | คำค้นหา         |
| page      | number | ❌       | 1       | หน้า            |
| size      | number | ❌       | 20      | จำนวนต่อหน้า    |
| lang      | string | ❌       | th      | ภาษา (th/in/en) |

### 🏠 หน้าหลัก / รายการซีรีส์

```http
GET /api/home?page={page}&size={size}&lang={lang}
```

### 👑 VIP / Theater

```http
GET /api/vip?lang={lang}
```

### 📄 รายละเอียดซีรีส์

```http
GET /api/detail/{bookId}/v2?lang={lang}
```

### 📋 รายการตอน

```http
GET /api/chapters/{bookId}?lang={lang}
```

> 💡 รองรับซีรีส์ที่มีมากกว่า 500 ตอน

### 📺 URL สตรีม

```http
GET /api/stream?bookId={bookId}&episode={episode}&lang={lang}
```

| Parameter | Type   | Required | Description |
| --------- | ------ | -------- | ----------- |
| bookId    | number | ✅       | ID ซีรีส์   |
| episode   | number | ✅       | หมายเลขตอน  |

### ⬇️ ดาวน์โหลดแบบ Batch

```http
GET /download/{bookId}?lang={lang}
```

> ⚠️ Rate limit: 5 request/นาที

### 🏷️ หมวดหมู่

```http
GET /api/categories?lang={lang}
GET /api/category/{id}?page={page}&size={size}&lang={lang}
```

### ⭐ แนะนำ

```http
GET /api/recommend?lang={lang}
```

### 💚 Health Check

```http
GET /health
```

---

## 📦 รูปแบบ Response

### ✅ Success Response

```json
{
  "success": true,
  "data": [...],
  "meta": {
    "timestamp": "2026-01-13T10:00:00.000Z",
    "pagination": {
      "page": 1,
      "size": 10,
      "hasMore": true
    }
  }
}
```

### ❌ Error Response

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "ต้องระบุพารามิเตอร์: keyword"
  },
  "meta": {
    "timestamp": "2026-01-13T10:00:00.000Z"
  }
}
```

### Error Codes

| Code                  | HTTP | Description          |
| --------------------- | ---- | -------------------- |
| `VALIDATION_ERROR`    | 400  | ข้อมูลไม่ถูกต้อง     |
| `NOT_FOUND`           | 404  | ไม่พบข้อมูล          |
| `RATE_LIMIT_EXCEEDED` | 429  | Request มากเกินไป    |
| `REQUEST_TIMEOUT`     | 408  | Request timeout      |
| `INTERNAL_ERROR`      | 500  | Server error         |

---

## 🗂️ โครงสร้างโปรเจค

```
dramabox-rest-api/
├── 📁 docs/
│   ├── 📁 api/             # API Documentation & Postman
│   ├── 📁 deployment/      # คู่มือการ Deploy
│   └── 📁 general/         # ข้อมูลทั่วไป
├── 📁 src/
│   ├── 📁 config/          # การตั้งค่าแอป
│   ├── 📁 controllers/     # Business Logic
│   ├── 📁 middlewares/     # Express Middlewares
│   ├── 📁 routes/          # API Routes
│   ├── 📁 services/        # Third-party Services
│   ├── 📁 utils/           # Utility Functions
│   ├── 📁 styles/          # Tailwind Source
│   └── 📄 app.js           # App Assembly
├── 📁 public/
│   └── 📁 css/             # Compiled CSS
├── 📁 views/
│   └── 📄 docs.ejs         # หน้าเอกสาร (ภาษาไทย)
├── 📄 server.js            # Entry Point
├── 📄 tailwind.config.js
└── 📄 package.json
```

---

## 🛠️ Scripts

```bash
npm start           # Production server
npm run dev         # Development with hot reload
npm run build:css   # Build Tailwind CSS
npm run watch:css   # Watch Tailwind changes
```

---

## 🚀 การ Deploy

เรามีคู่มือโดยละเอียดสำหรับแพลตฟอร์มต่างๆ:

- [**Vercel**](docs/deployment/VERCEL.md) (แนะนำสำหรับผู้เริ่มต้น)
- [**Shared Hosting (cPanel)**](docs/deployment/SHARED_HOSTING.md)
- [**VPS (Ubuntu/Debian)**](docs/deployment/VPS.md)
- [**aaPanel**](docs/deployment/AAPANEL.md)

### เพิ่มเติม

- [**Docker Guide**](docs/deployment/DOCKER.md) (เร็วๆ นี้)

---

## 📝 Changelog

### v1.3.0 (2026-01-13) - Thai Edition

- 🇹🇭 **UI ภาษาไทย**: แปลหน้าเอกสาร UI เป็นภาษาไทยทั้งหมด
- 🕐 **เวลาไทย**: แสดงเวลาตามโซนเวลาประเทศไทย (GMT+7)
- 📺 **รองรับ 500+ ตอน**: เพิ่มขีดจำกัดจำนวนตอนที่ดึงได้
- 🎨 **Font Niramit**: ใช้ฟอนต์ไทยสวยงาม
- � **SweetAlert**: แจ้งเตือนสถานะแบบสวยงาม
- 📱 **Responsive**: รองรับทุกขนาดหน้าจอ

### v1.2.0 (2024-12-30)

- ✅ Rate limiting (100 req/min)
- ✅ Gzip compression
- ✅ Helmet security headers
- ✅ Standardized response format
- ✅ Global error handling
- ✅ Graceful shutdown
- ✅ Health check endpoint
- ✅ Instance pooling

### v1.1.0

- ✅ Retry logic with exponential backoff
- ✅ Response caching (node-cache)
- ✅ Better error messages
- ✅ Tailwind CSS (local build)
- ✅ Modern documentation UI

### v1.0.0

- 🎉 Initial release

---

## 👨‍💻 ผู้พัฒนา

**Original by Handoko**

[![GitHub](https://img.shields.io/badge/GitHub-hndko-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/hndko)

---

## 📄 License

MIT License - สามารถใช้งานได้ทั้งโปรเจคส่วนตัวและเชิงพาณิชย์

---

<div align="center">

**⭐ กด Star ถ้าคุณชอบโปรเจคนี้!**

Made with ❤️ Thai Edition ��

</div>
