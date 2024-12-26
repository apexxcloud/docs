File Operations
Upload File
Path: /api/v1/files/upload
Method: POST
Query Parameters: bucket_name, region, visibility
Delete File
Path: /api/v1/files/delete
Method: DELETE
Query Parameters: bucket_name, region, key
Get Signed URL
Path: /api/v1/files/signed-url
Method: GET
Query Parameters: bucket_name, region, key, expires_in
Multipart Upload Operations
Start Multipart Upload
Path: /api/v1/files/multipart/start
Method: POST
Query Parameters: bucket_name, region, key, total_parts, mime_type, visibility
5. Upload Part
Path: /api/v1/files/multipart/{uploadId}/upload
Method: POST
Query Parameters: bucket_name, region, part_number, key, total_parts
Complete Multipart Upload
Path: /api/v1/files/multipart/{uploadId}/complete
Method: POST
Query Parameters: bucket_name, region, key
Cancel Multipart Upload
Path: /api/v1/files/multipart/{uploadId}/cancel
Method: POST
Query Parameters: bucket_name, region, key
Bucket Operations
List Bucket Contents
Path: /api/v1/files/list
Method: GET
Query Parameters: bucket_name, region, prefix, page, limit
All APIs include authentication headers:
X-Access-Key
X-Signature
X-Timestamp
Base URL: https://api.apexxcloud.com

### File Operations

#### 1. Upload File
**Success Response (200)**
```json
{
  "data": {
    "message": "File uploaded successfully",
    "key": "example.jpg",
    "location": "https://cdn.apexxcloud.com/f/abc123/example.jpg",
    "bucket": "my-bucket",
    "region": "us-east-1",
    "visibility": "public",
    "size": 1024567,
    "content_type": "image/jpeg"
  }
}
```

**Error Response (400/404/500)**
```json
{
  "error": "Validation error message" | "Bucket not found" | "Internal server error"
}
```

#### 2. Delete File
**Success Response (200)**
```json
{
  "data": {
    "message": "File and all transformations deleted successfully"
  }
}
```

**Error Response (400/404/500)**
```json
{
  "error": "Validation error message" | "File/Bucket not found" | "Internal server error"
}
```

#### 3. Get Signed URL
**Success Response (200)**
```json
{
  "data": {
    "url": "https://cdn.apexxcloud.com/f/abc123/example.jpg?signature=xyz789",
    "expires_in": 3600,
    "is_public": false
  }
}
```

**For Public Files**
```json
{
  "data": {
    "url": "https://cdn.apexxcloud.com/f/abc123/example.jpg",
    "is_public": true
  }
}
```

### Multipart Upload Operations

#### 1. Start Multipart Upload
**Success Response (200)**
```json
{
  "data": {
    "uploadId": "12345abc",
    "key": "large-file.zip",
    "bucket": "my-bucket",
    "fileId": "60a12345b678cd9012345678"
  }
}
```

#### 2. Upload Part
**Success Response (200)**
```json
{
  "data": {
    "partNumber": 1,
    "ETag": "\"abc123def456\""
  }
}
```

#### 3. Complete Multipart Upload
**Success Response (200)**
```json
{
  "data": {
    "message": "Multipart upload completed successfully",
    "key": "large-file.zip",
    "location": "https://cdn.apexxcloud.com/f/abc123/large-file.zip",
    "size": 104857600,
    "bucket": "my-bucket",
    "region": "us-east-1",
    "visibility": "public",
    "content_type": "application/zip"
  }
}
```

#### 4. Cancel Multipart Upload
**Success Response (200)**
```json
{
  "data": {
    "message": "Multipart upload cancelled successfully"
  }
}
```

### Bucket Operations

#### List Bucket Contents
**Success Response (200)**
```json
{
  "data": {
    "contents": {
      "name": "/",
      "type": "folder",
      "path": "",
      "children": [
        {
          "name": "example.jpg",
          "type": "file",
          "path": "example.jpg",
          "size": 1024567,
          "content_type": "image/jpeg",
          "last_modified": "2024-03-20T10:30:00Z",
          "visibility": "public"
        },
        {
          "name": "documents",
          "type": "folder",
          "path": "documents/",
          "size": null,
          "last_modified": null
        }
      ]
    },
    "pagination": {
      "current_page": 1,
      "total_pages": 5,
      "total_items": 100,
      "items_per_page": 20
    }
  }
}
```

### Common Error Responses

#### Authentication Error (401)
```json
{
  "error": "Invalid authentication credentials"
}
```

#### Validation Error (400)
```json
{
  "error": "Validation failed",
  "details": [
    {
      "field": "bucket_name",
      "message": "Bucket name is required"
    }
  ]
}
```

#### Not Found Error (404)
```json
{
  "error": "Resource not found",
  "message": "Bucket or file not found"
}
```

#### Server Error (500)
```json
{
  "error": "Internal server error"
}
```

### Notes:
1. All error responses follow a consistent format with an `error` field
2. Success responses always contain a `data` object
3. Pagination is included where applicable
4. Timestamps are in ISO 8601 format
5. File sizes are in bytes
6. URLs are HTTPS by default
