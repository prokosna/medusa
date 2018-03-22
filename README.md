# Medusa

This is the experimental project to manage entry and exit of a room by WebCam.

Medusa is consist of several micro services:

- [Eye](https://github.com/prokosna/medusa_eye)
  - Controlling WebCam and sending images to the following service

- [Synapse](https://github.com/prokosna/medusa_synapse)
  - Simple API server that receives images from Eye and push them to a Kafka cluster
  
- [Brain](https://github.com/prokosna/medusa_brain)
  - Consuming and analyzing images. When some events are detected, publish them to a RabbitMQ cluster
  
- [Gazer](https://github.com/prokosna/medusa_gazer)
  - Subscribing events and providing API for a dashboard
 
- [Stones](https://github.com/prokosna/medusa_stones)
  - Dashboard (SPA hosted by nginx)


## Overview

```
+-----------------------+
|                       |
|      Eye (WebCam)     |
|                       |
+-----------+-----------+
            |
            |  HTTP
            v
+-----------+-----------+
|                       |
|      Synapse (API)    |
|                       |
+-----------+-----------+
            |
            |
            v
+-----------+-----------+
|                       |
|         Kafka         |
|                       |
+-----------+-----------+
            |
            |
            v
+-----------+-----------+
|                       |
| Brain(Image Analaysis)|
|                       |
+-----------+-----------+
            |
            |  AMQP
            v
+-----------+-----------+
|                       |
|       RabbitMQ        |
|                       |
+--------+-----+--------+
         ^     |
         |     |  AMQP
         |     v
+--------+-----+--------+     +--------------+
|                       +---->+              |
|      Gazer (API)      |     |    MongoDB   |
|                       +<----+              |
+-----------+-----------+     +--------------+
            ^
            |  HTTP
            |
+-----------+-----------+
|                       |
|   Stones (Dashboard)  |
|                       |
+-----------------------+

```