# Complete In-Memory Blog API - Usage Examples & Test Cases

## 🚀 QUICK START COMMANDS

```bash
# 1. Create virtual environment
python -m venv venv
source venv/bin/activate

# 2. Install dependencies
pip install -r IN_MEMORY_FINAL_requirements.txt

# 3. Run migrations
python manage.py migrate

# 4. Run server
python manage.py runserver
```

Access at: http://127.0.0.1:8000/swagger/

---

## 📝 CURL EXAMPLES

### ═══ CREATE USER ═══
```bash
curl -X POST http://127.0.0.1:8000/api/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "john_doe",
    "email": "john@example.com",
    "first_name": "John",
    "last_name": "Doe",
    "bio": "Python developer"
  }'
```

Response (201 Created):
```json
{
  "id": 1,
  "username": "john_doe",
  "email": "john@example.com",
  "first_name": "John",
  "last_name": "Doe",
  "bio": "Python developer",
  "is_active": true,
  "created_at": "2024-01-15T10:30:00Z",
  "updated_at": "2024-01-15T10:30:00Z"
}
```

### ═══ CREATE BLOG ═══
```bash
curl -X POST http://127.0.0.1:8000/api/blogs/ \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Learning Django REST Framework",
    "content": "This is a comprehensive guide to building REST APIs with Django REST Framework. In this tutorial, we will cover all the basics and advanced concepts needed to build scalable APIs.",
    "author_id": 1,
    "is_published": true
  }'
```

Response (201 Created):
```json
{
  "id": 1,
  "title": "Learning Django REST Framework",
  "slug": "learning-django-rest-framework",
  "content": "This is a comprehensive guide...",
  "author_id": 1,
  "author_details": {
    "id": 1,
    "username": "john_doe",
    "email": "john@example.com",
    "first_name": "John",
    "last_name": "Doe"
  },
  "author_name": "John Doe",
  "is_published": true,
  "views_count": 0,
  "word_count": 23,
  "created_at": "2024-01-15T10:30:00Z",
  "updated_at": "2024-01-15T10:30:00Z"
}
```

### ═══ LIST USERS WITH PAGINATION ═══
```bash
curl "http://127.0.0.1:8000/api/users/?page=1&page_size=10"
```

Response:
```json
{
  "count": 5,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": 1,
      "username": "john_doe",
      "email": "john@example.com",
      "first_name": "John",
      "last_name": "Doe",
      "is_active": true,
      "created_at": "2024-01-15T10:30:00Z"
    }
  ]
}
```

### ═══ SEARCH USERS ═══
```bash
curl "http://127.0.0.1:8000/api/users/?search=john"
```

### ═══ LIST BLOGS WITH FILTERING ═══
```bash
# All published blogs
curl "http://127.0.0.1:8000/api/blogs/?is_published=true"

# Blogs by specific author
curl "http://127.0.0.1:8000/api/blogs/?author_id=1"

# Blogs with minimum views
curl "http://127.0.0.1:8000/api/blogs/?min_views=5"

# Combination: published blogs by author with ordering
curl "http://127.0.0.1:8000/api/blogs/?author_id=1&is_published=true&ordering=-views_count"
```

### ═══ SEARCH BLOGS ═══
```bash
curl "http://127.0.0.1:8000/api/blogs/?search=django"
```

### ═══ GET TRENDING BLOGS ═══
```bash
# Top 5 most viewed
curl "http://127.0.0.1:8000/api/blogs/trending/?limit=5"

# Top 10
curl "http://127.0.0.1:8000/api/blogs/trending/?limit=10"
```

### ═══ GET RECENT BLOGS ═══
```bash
curl "http://127.0.0.1:8000/api/blogs/recent/?limit=5"
```

### ═══ GET USER BLOGS ═══
```bash
curl "http://127.0.0.1:8000/api/users/1/blogs/"
```

Response:
```json
{
  "username": "john_doe",
  "total_blogs": 2,
  "blogs": [
    {
      "id": 1,
      "title": "First Blog",
      "slug": "first-blog",
      "content": "...",
      "author_id": 1,
      "is_published": true,
      "views_count": 5,
      "created_at": "2024-01-15T10:30:00Z",
      "updated_at": "2024-01-15T10:30:00Z"
    }
  ]
}
```

### ═══ GET BLOG STATISTICS ═══
```bash
curl "http://127.0.0.1:8000/api/blogs/count/"
```

Response:
```json
{
  "total_blogs": 10,
  "published_blogs": 8,
  "draft_blogs": 2
}
```

### ═══ GET USER STATISTICS ═══
```bash
curl "http://127.0.0.1:8000/api/users/count/"
```

Response:
```json
{
  "total_users": 5,
  "active_users": 4,
  "inactive_users": 1
}
```

### ═══ PUBLISH A BLOG ═══
```bash
curl -X POST http://127.0.0.1:8000/api/blogs/1/publish/
```

### ═══ UNPUBLISH A BLOG ═══
```bash
curl -X POST http://127.0.0.1:8000/api/blogs/1/unpublish/
```

### ═══ UPDATE BLOG (PARTIAL) ═══
```bash
curl -X PATCH http://127.0.0.1:8000/api/blogs/1/ \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Updated Title"
  }'
```

### ═══ DELETE BLOG ═══
```bash
curl -X DELETE http://127.0.0.1:8000/api/blogs/1/
```

### ═══ ACTIVATE USER ═══
```bash
curl -X POST http://127.0.0.1:8000/api/users/1/activate/
```

### ═══ DEACTIVATE USER ═══
```bash
curl -X POST http://127.0.0.1:8000/api/users/1/deactivate/
```

---

## 🐍 PYTHON REQUESTS EXAMPLES

```python
import requests
import json

BASE_URL = "http://127.0.0.1:8000/api"

# ═══ CREATE USER ═══
user_data = {
    "username": "jane_doe",
    "email": "jane@example.com",
    "first_name": "Jane",
    "last_name": "Doe",
    "bio": "Data scientist"
}

response = requests.post(f"{BASE_URL}/users/", json=user_data)
print(response.json())  # See new user

# ═══ CREATE BLOG ═══
blog_data = {
    "title": "Introduction to Python",
    "content": "Python is a powerful programming language used for web development, data science, AI, and more. It has a simple syntax that makes it easy to learn.",
    "author_id": 1,
    "is_published": True
}

response = requests.post(f"{BASE_URL}/blogs/", json=blog_data)
blog = response.json()
print(f"Created blog: {blog['title']}")

# ═══ GET ALL BLOGS ═══
response = requests.get(f"{BASE_URL}/blogs/")
data = response.json()
print(f"Total blogs: {data['count']}")
print(f"Results: {data['results']}")

# ═══ SEARCH BLOGS ═══
response = requests.get(f"{BASE_URL}/blogs/?search=python")
blogs = response.json()['results']
print(f"Found {len(blogs)} blogs about Python")

# ═══ FILTER BY AUTHOR ═══
response = requests.get(f"{BASE_URL}/blogs/?author_id=1&ordering=-created_at")
blogs = response.json()['results']
print(f"Author 1 has {len(blogs)} blogs")

# ═══ GET TRENDING BLOGS ═══
response = requests.get(f"{BASE_URL}/blogs/trending/?limit=5")
trending = response.json()['trending_blogs']
print(f"Top blogs: {trending}")

# ═══ UPDATE BLOG ═══
update_data = {"title": "Updated Title"}
response = requests.patch(f"{BASE_URL}/blogs/1/", json=update_data)
print(f"Updated: {response.json()['title']}")

# ═══ PUBLISH BLOG ═══
response = requests.post(f"{BASE_URL}/blogs/1/publish/")
print(response.json()['message'])

# ═══ DELETE BLOG ═══
response = requests.delete(f"{BASE_URL}/blogs/1/")
print(f"Deleted. Status: {response.status_code}")
```

---

## 📋 PAGINATION EXAMPLES

```bash
# First page (default 10 items)
curl "http://127.0.0.1:8000/api/blogs/"

# Second page
curl "http://127.0.0.1:8000/api/blogs/?page=2"

# 20 items per page
curl "http://127.0.0.1:8000/api/blogs/?page_size=20"

# Page 2 with 20 items
curl "http://127.0.0.1:8000/api/blogs/?page=2&page_size=20"
```

---

## 🔍 FILTERING COMBINATIONS

```bash
# Published blogs by author 1, sorted by views
curl "http://127.0.0.1:8000/api/blogs/?author_id=1&is_published=true&ordering=-views_count&page_size=20"

# Blogs with 5-100 views, sorted alphabetically
curl "http://127.0.0.1:8000/api/blogs/?min_views=5&max_views=100&ordering=title"

# Search + Filter: Python blogs by author 2
curl "http://127.0.0.1:8000/api/blogs/?search=python&author_id=2"

# Complex: Published, author 1, 10+ views, sorted by date
curl "http://127.0.0.1:8000/api/blogs/?is_published=true&author_id=1&min_views=10&ordering=-created_at"
```

---

## ✅ TESTING CHECKLIST

- [ ] Create 2 users
- [ ] Create 5 blogs (mix published/draft)
- [ ] Test pagination with page_size=2
- [ ] Search for blogs with keyword
- [ ] Filter by author
- [ ] Filter by published status
- [ ] Sort by views descending
- [ ] Get trending blogs
- [ ] Get recent blogs
- [ ] Update a blog
- [ ] Delete a blog
- [ ] Deactivate a user
- [ ] Test error handling (create invalid blog)

---

## 📊 DATA EXAMPLES

### Sample User
```json
{
  "id": 1,
  "username": "john_developer",
  "email": "john@dev.com",
  "first_name": "John",
  "last_name": "Developer",
  "bio": "Full-stack developer with 10 years experience",
  "is_active": true,
  "created_at": "2024-01-15T10:30:00Z",
  "updated_at": "2024-01-15T10:30:00Z"
}
```

### Sample Blog
```json
{
  "id": 1,
  "title": "Django Best Practices",
  "slug": "django-best-practices",
  "content": "When building Django applications, follow these best practices: use virtual environments, write tests, use class-based views, implement proper logging, use middleware wisely, and always validate input.",
  "author_id": 1,
  "author_details": {
    "id": 1,
    "username": "john_developer",
    "email": "john@dev.com",
    "first_name": "John",
    "last_name": "Developer"
  },
  "author_name": "John Developer",
  "is_published": true,
  "views_count": 42,
  "word_count": 35,
  "created_at": "2024-01-15T10:30:00Z",
  "updated_at": "2024-01-15T10:30:00Z"
}
```

---

## 🎯 COMMON QUERIES

### Get latest 10 published blogs
```bash
curl "http://127.0.0.1:8000/api/blogs/?is_published=true&ordering=-created_at&page_size=10"
```

### Get top 5 most viewed blogs
```bash
curl "http://127.0.0.1:8000/api/blogs/trending/?limit=5"
```

### Search Django blogs by specific author
```bash
curl "http://127.0.0.1:8000/api/blogs/?search=django&author_id=1"
```

### Get all users, sorted by name
```bash
curl "http://127.0.0.1:8000/api/users/?ordering=username&page_size=50"
```

### Get active users only (paginated)
```bash
curl "http://127.0.0.1:8000/api/users/?page=1&page_size=20"
```

---

## 💡 TIPS

1. **Swagger UI**: Use http://127.0.0.1:8000/swagger/ to test endpoints interactively
2. **ReDoc**: View comprehensive API docs at http://127.0.0.1:8000/redoc/
3. **Pagination**: Default is 10 items per page, adjust with ?page_size=20
4. **Ordering**: Prefix with `-` for descending (e.g., -created_at for newest first)
5. **Search**: Works across multiple fields (title, content, author name, etc.)
6. **Filters**: Combine multiple filters with `&`