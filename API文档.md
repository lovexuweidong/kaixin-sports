# StoryFlow API 文档

**版本：** v1.0  
**基础URL：** `https://api.storyflow.com/api/v1`  
**认证方式：** Bearer Token (JWT)

---

## 目录

1. [认证 API](#认证-api)
2. [小说 API](#小说-api)
3. [用户 API](#用户-api)
4. [阅读 API](#阅读-api)
5. [推荐 API](#推荐-api)
6. [统计 API](#统计-api)
7. [错误处理](#错误处理)

---

## 认证 API

### 注册

**请求**
```
POST /auth/register
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "secure_password_123",
  "username": "john_doe",
  "language": "zh-TW"
}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "message": "success",
  "data": {
    "user_id": 12345,
    "username": "john_doe",
    "email": "user@example.com",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expires_in": 604800
  }
}
```

**错误响应 (400 Bad Request)**
```json
{
  "code": 400,
  "message": "Email already exists",
  "error": "DUPLICATE_EMAIL"
}
```

---

### 登录

**请求**
```
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "secure_password_123"
}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "message": "success",
  "data": {
    "user_id": 12345,
    "username": "john_doe",
    "subscription_tier": "free",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expires_in": 604800
  }
}
```

---

### 刷新Token

**请求**
```
POST /auth/refresh
Content-Type: application/json

{
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expires_in": 604800
  }
}
```

---

### 登出

**请求**
```
POST /auth/logout
Authorization: Bearer {token}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "message": "success"
}
```

---

## 小说 API

### 获取小说列表

**请求**
```
GET /stories?page=1&limit=20&category=悬疑&language=zh-TW&sort=-published_at
```

**查询参数**
| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| page | int | 否 | 页码，默认1 |
| limit | int | 否 | 每页数量，默认20，最大100 |
| category | string | 否 | 分类筛选（悬疑,爱情,奇幻,都市,恐怖） |
| language | string | 否 | 语言（zh-TW,en），默认zh-TW |
| sort | string | 否 | 排序（-published_at,-views,-rating,+created_at） |
| search | string | 否 | 搜索关键词 |

**响应 (200 OK)**
```json
{
  "code": 0,
  "message": "success",
  "data": {
    "items": [
      {
        "id": "story_001",
        "title": "午夜的秘密",
        "slug": "midnight-secret",
        "category": "悬疑",
        "language": "zh-TW",
        "summary": "一个普通上班族接到一通神秘电话...",
        "cover": "https://cdn.storyflow.com/covers/story_001.jpg",
        "author": "AI-StoryFlow",
        "word_count": 3000,
        "chapter_count": 3,
        "rating": 4.8,
        "views": 12300,
        "likes": 2100,
        "published_at": "2026-03-20T10:30:00Z"
      },
      {
        "id": "story_002",
        "title": "失落的城市",
        "slug": "lost-city",
        "category": "奇幻",
        "language": "zh-TW",
        "summary": "在一个被遗忘的古城中...",
        "cover": "https://cdn.storyflow.com/covers/story_002.jpg",
        "author": "AI-StoryFlow",
        "word_count": 3500,
        "chapter_count": 4,
        "rating": 4.6,
        "views": 8900,
        "likes": 1500,
        "published_at": "2026-03-19T14:20:00Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 1250,
      "pages": 63
    }
  }
}
```

---

### 获取单个小说

**请求**
```
GET /stories/{story_id}
Authorization: Bearer {token}  (可选)
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "id": "story_001",
    "title": "午夜的秘密",
    "slug": "midnight-secret",
    "category": "悬疑",
    "language": "zh-TW",
    "summary": "一个普通上班族接到一通神秘电话，他的人生从此改变...",
    "cover": "https://cdn.storyflow.com/covers/story_001.jpg",
    "author": "AI-StoryFlow",
    "word_count": 3000,
    "chapter_count": 3,
    "rating": 4.8,
    "views": 12300,
    "likes": 2100,
    "comments_count": 450,
    "published_at": "2026-03-20T10:30:00Z",
    "chapters": [
      {
        "number": 1,
        "title": "诡异的来电",
        "content": "李明接起电话的那一刻，他不知道自己的人生即将发生翻天覆地的变化...",
        "word_count": 1000,
        "unlock_type": "free"
      },
      {
        "number": 2,
        "title": "真相浮出水面",
        "content": "电话那端传来一个陌生的女性声音...",
        "word_count": 1000,
        "unlock_type": "free"
      },
      {
        "number": 3,
        "title": "反转的结局",
        "content": "[需要解锁]",
        "word_count": 1000,
        "unlock_type": "video"
      }
    ],
    "user_liked": false,
    "user_progress": {
      "read_words": 1500,
      "completed": false,
      "last_read_at": "2026-03-23T10:30:00Z"
    }
  }
}
```

---

### 获取下一篇小说（用于左右滑动）

**请求**
```
GET /stories/{story_id}/next?category=悬疑
Authorization: Bearer {token}
```

**查询参数**
| 参数 | 类型 | 说明 |
|------|------|------|
| category | string | 按分类获取下一篇（可选） |

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "id": "story_003",
    "title": "雨夜的邂逅",
    "slug": "rainy-night-encounter",
    "category": "爱情",
    "summary": "...",
    "cover": "https://cdn.storyflow.com/covers/story_003.jpg",
    "rating": 4.7,
    "views": 9800,
    "likes": 1800
  }
}
```

---

### 点赞/取消点赞

**请求**
```
POST /stories/{story_id}/like
Authorization: Bearer {token}
Content-Type: application/json

{
  "action": "like"  // "like" 或 "unlike"
}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "liked": true,
    "likes_count": 2101
  }
}
```

---

### 获取小说分类

**请求**
```
GET /stories/categories?language=zh-TW
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": [
    {
      "id": "suspense",
      "name": "悬疑",
      "name_en": "Suspense",
      "count": 320,
      "icon": "🔍"
    },
    {
      "id": "romance",
      "name": "爱情",
      "name_en": "Romance",
      "count": 280,
      "icon": "💕"
    },
    {
      "id": "fantasy",
      "name": "奇幻",
      "name_en": "Fantasy",
      "count": 250,
      "icon": "✨"
    },
    {
      "id": "urban",
      "name": "都市",
      "name_en": "Urban",
      "count": 200,
      "icon": "🏙️"
    },
    {
      "id": "horror",
      "name": "恐怖",
      "name_en": "Horror",
      "count": 150,
      "icon": "👻"
    }
  ]
}
```

---

## 用户 API

### 获取当前用户信息

**请求**
```
GET /users/me
Authorization: Bearer {token}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "id": 12345,
    "username": "john_doe",
    "email": "user@example.com",
    "avatar_url": "https://cdn.storyflow.com/avatars/12345.jpg",
    "language": "zh-TW",
    "subscription_tier": "free",
    "subscription_expires_at": null,
    "created_at": "2026-01-15T00:00:00Z",
    "updated_at": "2026-03-23T10:30:00Z",
    "stats": {
      "total_read_words": 45230,
      "total_stories_read": 12,
      "total_likes": 8,
      "reading_streak": 7
    }
  }
}
```

---

### 更新用户信息

**请求**
```
PUT /users/me
Authorization: Bearer {token}
Content-Type: application/json

{
  "username": "new_username",
  "avatar_url": "https://...",
  "language": "en"
}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "id": 12345,
    "username": "new_username",
    "email": "user@example.com",
    "language": "en",
    "updated_at": "2026-03-23T11:00:00Z"
  }
}
```

---

### 修改密码

**请求**
```
POST /users/me/change-password
Authorization: Bearer {token}
Content-Type: application/json

{
  "old_password": "old_password_123",
  "new_password": "new_password_456"
}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "message": "Password changed successfully"
}
```

---

### 获取用户书架

**请求**
```
GET /users/me/bookshelf?sort=-read_at&limit=20
Authorization: Bearer {token}
```

**查询参数**
| 参数 | 类型 | 说明 |
|------|------|------|
| sort | string | 排序（-read_at,-created_at,+title） |
| limit | int | 每页数量 |

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "items": [
      {
        "story_id": "story_001",
        "title": "午夜的秘密",
        "cover": "https://cdn.storyflow.com/covers/story_001.jpg",
        "category": "悬疑",
        "progress": 35,
        "read_words": 1050,
        "total_words": 3000,
        "last_read_at": "2026-03-23T10:30:00Z",
        "completed": false
      },
      {
        "story_id": "story_002",
        "title": "失落的城市",
        "cover": "https://cdn.storyflow.com/covers/story_002.jpg",
        "category": "奇幻",
        "progress": 100,
        "read_words": 3500,
        "total_words": 3500,
        "last_read_at": "2026-03-22T15:20:00Z",
        "completed": true
      }
    ],
    "pagination": {
      "total": 12,
      "limit": 20
    }
  }
}
```

---

### 获取用户收藏

**请求**
```
GET /users/me/likes?limit=20&page=1
Authorization: Bearer {token}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "items": [
      {
        "story_id": "story_001",
        "title": "午夜的秘密",
        "cover": "https://cdn.storyflow.com/covers/story_001.jpg",
        "rating": 4.8,
        "liked_at": "2026-03-23T10:30:00Z"
      }
    ],
    "pagination": {
      "total": 8,
      "page": 1,
      "limit": 20
    }
  }
}
```

---

## 阅读 API

### 保存阅读进度

**请求**
```
POST /reading-history
Authorization: Bearer {token}
Content-Type: application/json

{
  "story_id": "story_001",
  "chapter_number": 1,
  "scroll_position": 1500,
  "completed": false
}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "message": "Progress saved successfully"
}
```

---

### 获取阅读历史

**请求**
```
GET /reading-history?limit=20&page=1
Authorization: Bearer {token}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "items": [
      {
        "story_id": "story_001",
        "title": "午夜的秘密",
        "cover": "https://cdn.storyflow.com/covers/story_001.jpg",
        "scroll_position": 1500,
        "read_at": "2026-03-23T10:30:00Z",
        "progress": 50
      }
    ],
    "pagination": {
      "total": 12,
      "page": 1,
      "limit": 20
    }
  }
}
```

---

### 检查解锁状态

**请求**
```
GET /stories/{story_id}/unlock-status?chapter_number=3
Authorization: Bearer {token}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "story_id": "story_001",
    "chapter_number": 3,
    "unlocked": false,
    "unlock_options": [
      {
        "method": "video",
        "duration": 30,
        "available": true,
        "description": "看30秒广告视频"
      },
      {
        "method": "subscription",
        "available": false,
        "reason": "not_subscribed",
        "description": "升级到高级会员"
      },
      {
        "method": "paid",
        "price": 0.99,
        "currency": "USD",
        "available": true,
        "description": "购买单章"
      }
    ]
  }
}
```

---

### 解锁章节

**请求**
```
POST /stories/{story_id}/unlock
Authorization: Bearer {token}
Content-Type: application/json

{
  "chapter_number": 3,
  "unlock_method": "video",
  "video_id": "ad_12345"
}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "unlocked": true,
    "content": "反转的结局内容...",
    "unlock_expires_at": null
  }
}
```

---

## 推荐 API

### 获取个性化推荐

**请求**
```
GET /recommendations?limit=20&category=悬疑
Authorization: Bearer {token}
```

**查询参数**
| 参数 | 类型 | 说明 |
|------|------|------|
| limit | int | 推荐数量，默认20 |
| category | string | 按分类推荐（可选） |

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "items": [
      {
        "story_id": "story_003",
        "title": "诡异的访客",
        "cover": "https://cdn.storyflow.com/covers/story_003.jpg",
        "category": "悬疑",
        "rating": 4.7,
        "reason": "similar_to_午夜的秘密",
        "score": 0.92
      },
      {
        "story_id": "story_004",
        "title": "午夜的秘密2",
        "cover": "https://cdn.storyflow.com/covers/story_004.jpg",
        "category": "悬疑",
        "rating": 4.6,
        "reason": "popular_in_category",
        "score": 0.88
      }
    ]
  }
}
```

---

### 获取热门小说

**请求**
```
GET /stories/trending?limit=10&period=week
```

**查询参数**
| 参数 | 类型 | 说明 |
|------|------|------|
| limit | int | 数量 |
| period | string | 时间段（day,week,month） |

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "items": [
      {
        "rank": 1,
        "story_id": "story_001",
        "title": "午夜的秘密",
        "views_increase": 5230,
        "likes_increase": 450
      }
    ]
  }
}
```

---

## 统计 API

### 获取用户统计

**请求**
```
GET /users/me/stats
Authorization: Bearer {token}
```

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "total_read_words": 45230,
    "total_stories_read": 12,
    "total_stories_completed": 5,
    "total_likes": 8,
    "reading_streak": 7,
    "reading_time_today": 45,
    "reading_time_week": 320,
    "reading_time_month": 1200,
    "favorite_category": "悬疑",
    "achievements": [
      {
        "id": "first_story",
        "name": "初次阅读",
        "description": "完成第一篇小说",
        "unlocked_at": "2026-01-15T00:00:00Z"
      },
      {
        "id": "reading_streak_7",
        "name": "连续阅读7天",
        "description": "连续7天阅读",
        "unlocked_at": "2026-03-23T00:00:00Z"
      }
    ]
  }
}
```

---

### 获取管理员统计面板

**请求**
```
GET /admin/analytics?start_date=2026-03-01&end_date=2026-03-23&metric=daily_users
Authorization: Bearer {admin_token}
```

**查询参数**
| 参数 | 类型 | 说明 |
|------|------|------|
| start_date | string | 开始日期 (YYYY-MM-DD) |
| end_date | string | 结束日期 (YYYY-MM-DD) |
| metric | string | 指标类型 |

**响应 (200 OK)**
```json
{
  "code": 0,
  "data": {
    "daily_users": [
      {
        "date": "2026-03-01",
        "value": 1250,
        "change": 5.2
      },
      {
        "date": "2026-03-02",
        "value": 1380,
        "change": 10.4
      }
    ],
    "summary": {
      "total_users": 45230,
      "active_users_today": 3450,
      "new_users_today": 850,
      "total_stories": 1250,
      "total_views": 2450000,
      "total_revenue": 125000,
      "avg_session_duration": 12.5
    }
  }
}
```

---

## 错误处理

### 错误响应格式

```json
{
  "code": 400,
  "message": "Bad Request",
  "error": "INVALID_INPUT",
  "details": {
    "field": "email",
    "reason": "Invalid email format"
  }
}
```

### 常见错误码

| 错误码 | HTTP状态 | 说明 |
|--------|---------|------|
| 0 | 200 | 成功 |
| 400 | 400 | 请求参数错误 |
| 401 | 401 | 未授权（需要登录） |
| 403 | 403 | 禁止访问（权限不足） |
| 404 | 404 | 资源不存在 |
| 409 | 409 | 冲突（如邮箱已存在） |
| 429 | 429 | 请求过于频繁 |
| 500 | 500 | 服务器错误 |

### 常见错误类型

```
INVALID_INPUT - 输入参数无效
DUPLICATE_EMAIL - 邮箱已存在
DUPLICATE_USERNAME - 用户名已存在
INVALID_PASSWORD - 密码不符合要求
INVALID_CREDENTIALS - 邮箱或密码错误
TOKEN_EXPIRED - Token已过期
TOKEN_INVALID - Token无效
STORY_NOT_FOUND - 小说不存在
USER_NOT_FOUND - 用户不存在
UNAUTHORIZED - 未授权
RATE_LIMIT_EXCEEDED - 请求过于频繁
INTERNAL_SERVER_ERROR - 服务器内部错误
```

---

## 速率限制

所有API端点都受到速率限制保护：

- **未认证用户**：100请求/小时
- **认证用户**：1000请求/小时
- **管理员**：10000请求/小时

响应头中包含速率限制信息：
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1640000000
```

---

## 分页

所有列表API都支持分页：

```json
{
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 1250,
    "pages": 63
  }
}
```

---

## 时间格式

所有时间戳使用ISO 8601格式：
```
2026-03-23T10:30:00Z
```

---

## 示例代码

### JavaScript/TypeScript

```typescript
import axios from 'axios';

const api = axios.create({
  baseURL: 'https://api.storyflow.com/api/v1'
});

// 登录
async function login(email: string, password: string) {
  const response = await api.post('/auth/login', {
    email,
    password
  });
  
  const { token } = response.data.data;
  api.defaults.headers.common['Authorization'] = `Bearer ${token}`;
  
  return response.data.data;
}

// 获取小说列表
async function getStories(page = 1, limit = 20) {
  const response = await api.get('/stories', {
    params: { page, limit }
  });
  return response.data.data;
}

// 保存阅读进度
async function saveProgress(storyId: string, progress: any) {
  const response = await api.post('/reading-history', {
    story_id: storyId,
    ...progress
  });
  return response.data;
}
```

### Python

```python
import requests

class StoryFlowAPI:
    def __init__(self, base_url='https://api.storyflow.com/api/v1'):
        self.base_url = base_url
        self.session = requests.Session()
    
    def login(self, email, password):
        response = self.session.post(
            f'{self.base_url}/auth/login',
            json={'email': email, 'password': password}
        )
        data = response.json()['data']
        self.session.headers['Authorization'] = f"Bearer {data['token']}"
        return data
    
    def get_stories(self, page=1, limit=20):
        response = self.session.get(
            f'{self.base_url}/stories',
            params={'page': page, 'limit': limit}
        )
        return response.json()['data']
    
    def save_progress(self, story_id, progress):
        response = self.session.post(
            f'{self.base_url}/reading-history',
            json={'story_id': story_id, **progress}
        )
        return response.json()

# 使用示例
api = StoryFlowAPI()
api.login('user@example.com', 'password')
stories = api.get_stories()
```

---

**文档版本：** 1.0  
**最后更新：** 2026-03-23  
**维护者：** StoryFlow 团队
