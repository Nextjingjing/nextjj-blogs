---
title: Microservice ตอนที่ 2 Interprocess communication (IPC)
description: Microservice ตอนที่ 2 การสื่อสารภายใน
date: 2026-04-03
categories:
    - Blogs
tags:
    - Microservice
    - Backend
toc: true
---

# Interprocess communication (การสื่อสารภายใน)

> การสื่อสารระหว่าง Service นั้นเป็นหนึ่งในหัวใจหลักของ Microservice ทำให้เกิดการแลกเปลี่ยนข้อมูลซึ่งกันและกัน

## ชนิดของ Interprocess communication

แบ่งออกเป็น 2 ชนิดได้แก่

1. Synchronous remote procedure invocation
   - ตัวอย่างเช่น gRPC, REST เป็นต้น
   - Circuit breaker pattern
2. Asynchronous messaging 
   - ตัวอย่างเช่น Kafka, RabbitMQ เป็นต้น
   - Message Broker pattern
   - Outbox Pattern

## Synchronous remote procedure invocation

> **Important Note** 📝:
>
> - รอการตอบสนอง (Blocking)
> - Circuit breaker pattern

### ลักษณะ
- Blocking เมื่อ Client ทำการ Request ไปหา Server และจะถูก Block (ต้องรอ) จนกว่า Server
- one-to-one สื่อสารได้แค่ 1 คู่เท่านั้น

### ข้อดี
- ไม่มีปัญหาจู้จี้จุกจิกเหมือน Asynchronous messaging
- เหมาะสมงานที่ต้องการคำตอบทันที (Realtime) หรือความถูกต้องสูง

### ประเด็นสำคัญ
- **gRPC ประหยัด Bandwidth** มากกว่า REST
- ต้องมีการกำหนด Timeout ของการ Request
  - เพื่อป้องกัน Server ปลายทางตายแล้วต้นทางยังรอคนตาย
- Circuit breaker pattern
  - เพื่อป้องกัน Server ปลายทางตายแล้วต้นทางยังไปถามเรื่อยๆ
  - Circuit breaker เปรียบเสมือนสะพานไฟ ไว้กันไฟฟ้าลัดวงจร (Short Circuit) ถ้าในบริบทนี้หมายถึงป้องกันปลายทางตาย

#### Circuit breaker pattern