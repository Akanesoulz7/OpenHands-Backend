# 🚀 Fizzo.org Auto-Update Guide

Fitur automation untuk auto-update novel ke fizzo.org telah berhasil diintegrasikan ke backend OpenHands!

## ✅ **Status Implementasi**

- ✅ **Playwright Integration**: Browser automation dengan Chromium
- ✅ **API Endpoint**: `/api/fizzo-auto-update` dan `/api/fizzo-list-novel`
- ✅ **Authentication**: Login otomatis ke fizzo.org
- ✅ **Novel List**: Scraping daftar novel user
- ✅ **Novel Selection**: Pilih novel berdasarkan ID
- ✅ **Chapter Upload**: Auto-create dan publish chapter
- ✅ **Error Handling**: Comprehensive error handling
- ✅ **Validation**: Input validation untuk content length
- ✅ **HF Spaces Ready**: Optimized untuk deployment gratisan
- ✅ **Import Error Handling**: Fallback implementation jika module tidak tersedia
- ✅ **Error Handling** dan timeout management
- ✅ **Security** untuk credentials
- ✅ **Validation** content length (1,000-60,000 karakter)

## 📡 API Endpoints

### 1. Mendapatkan Daftar Novel

```
POST /api/fizzo-list-novel
Content-Type: application/json
```

#### Request Body

```json
{
  "email": "your_email@gmail.com",
  "password": "your_password"
}
```

#### Response Success

```json
{
  "success": true,
  "message": "Berhasil mendapatkan 3 novel",
  "data": [
    {
      "title": "Judul Novel Pertama",
      "id": "12345"
    },
    {
      "title": "Judul Novel Kedua",
      "id": "67890"
    },
    {
      "title": "Judul Novel Ketiga",
      "id": "54321"
    }
  ]
}
```

#### Response Error

```json
{
  "success": false,
  "error": "Login failed"
}
```

### 2. Upload Chapter Novel

```
POST /api/fizzo-auto-update
Content-Type: application/json
```

#### Request Body

```json
{
  "email": "your_email@gmail.com",
  "password": "your_password",
  "chapter_title": "Bab 28: Judul Chapter",
  "chapter_content": "Isi chapter novel yang panjang minimal 1000 karakter...",
  "novel_id": "12345"  // Optional, jika tidak diisi akan menggunakan novel default
}
```

#### Response Success

```json
{
  "success": true,
  "message": "Chapter berhasil diupload ke fizzo.org",
  "data": {
    "success": true,
    "message": "Chapter created successfully",
    "chapter_title": "Bab 28: Judul Chapter",
    "content_length": 2500,
    "published": true,
    "confirmed": true
  }
}
```

#### Response Error

```json
{
  "success": false,
  "error": "Login failed"
}
```

## 🔧 Cara Penggunaan

### 1. Via cURL

#### Mendapatkan Daftar Novel

```bash
curl -X POST "https://your-backend.hf.space/api/fizzo-list-novel" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "your_email@gmail.com",
    "password": "your_password"
  }'
```

#### Upload Chapter Novel

```bash
curl -X POST "https://your-backend.hf.space/api/fizzo-auto-update" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "your_email@gmail.com",
    "password": "your_password",
    "chapter_title": "Bab 28: Pertarungan Terakhir",
    "chapter_content": "Di tengah malam yang kelam, protagonis menghadapi musuh terbesarnya. Dengan tekad yang bulat, dia melangkah maju tanpa rasa takut. Pertarungan ini akan menentukan nasib seluruh kerajaan...",
    "novel_id": "12345"
  }'
```

### 2. Via JavaScript (Frontend)

```javascript
// Mendapatkan daftar novel
const getNovelList = async () => {
  const response = await fetch('/api/fizzo-list-novel', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      email: 'your_email@gmail.com',
      password: 'your_password'
    })
  });
  
  const result = await response.json();
  
  if (result.success) {
    console.log(`✅ Berhasil mendapatkan ${result.data.length} novel!`);
    // Tampilkan daftar novel untuk dipilih user
    const novels = result.data;
    return novels;
  } else {
    console.error('❌ Gagal mendapatkan daftar novel:', result.error);
    return [];
  }
};

// Upload chapter ke novel tertentu
const updateNovel = async (novelId) => {
  const response = await fetch('/api/fizzo-auto-update', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      email: 'your_email@gmail.com',
      password: 'your_password',
      chapter_title: 'Bab 28: Pertarungan Terakhir',
      chapter_content: 'Isi chapter yang panjang...',
      novel_id: novelId // ID novel yang dipilih
    })
  });
  
  const result = await response.json();
  
  if (result.success) {
    console.log('✅ Chapter berhasil diupload!');
    console.log(result.data);
  } else {
    console.error('❌ Upload gagal:', result.error);
  }
};

// Contoh workflow lengkap
const completeWorkflow = async () => {
  // 1. Dapatkan daftar novel
  const novels = await getNovelList();
  
  if (novels.length > 0) {
    // 2. Pilih novel pertama (atau tampilkan UI untuk memilih)
    const selectedNovel = novels[0];
    console.log(`📚 Novel dipilih: ${selectedNovel.title} (ID: ${selectedNovel.id})`);
    
    // 3. Upload chapter ke novel yang dipilih
    await updateNovel(selectedNovel.id);
  }
};
```

### 3. Via Python

```python
import requests

def get_novel_list(email, password):
    """Mendapatkan daftar novel user"""
    url = "https://your-backend.hf.space/api/fizzo-list-novel"
    
    payload = {
        "email": email,
        "password": password
    }
    
    response = requests.post(url, json=payload)
    result = response.json()
    
    if result.get("success"):
        novels = result["data"]
        print(f"✅ Berhasil mendapatkan {len(novels)} novel!")
        for i, novel in enumerate(novels):
            print(f"{i+1}. {novel['title']} (ID: {novel['id']})")
        return novels
    else:
        print(f"❌ Gagal mendapatkan daftar novel: {result.get('error')}")
        return []

def upload_chapter(email, password, title, content, novel_id=None):
    """Upload chapter ke novel tertentu"""
    url = "https://your-backend.hf.space/api/fizzo-auto-update"
    
    payload = {
        "email": email,
        "password": password,
        "chapter_title": title,
        "chapter_content": content
    }
    
    # Tambahkan novel_id jika ada
    if novel_id:
        payload["novel_id"] = novel_id
    
    response = requests.post(url, json=payload)
    result = response.json()
    
    if result.get("success"):
        print("✅ Chapter berhasil diupload!")
        return result["data"]
    else:
        print(f"❌ Upload gagal: {result.get('error')}")
        return None

# Contoh workflow lengkap
def complete_workflow(email, password, chapter_title, chapter_content):
    # 1. Dapatkan daftar novel
    novels = get_novel_list(email, password)
    
    if novels:
        # 2. Pilih novel (misalnya novel pertama)
        selected_novel = novels[0]
        print(f"📚 Novel dipilih: {selected_novel['title']} (ID: {selected_novel['id']})")
        
        # 3. Upload chapter ke novel yang dipilih
        upload_chapter(
            email=email,
            password=password, 
            title=chapter_title,
            content=chapter_content,
            novel_id=selected_novel['id']
        )
    else:
        print("❌ Tidak ada novel ditemukan atau gagal login")

# Contoh penggunaan
complete_workflow(
    email="your_email@gmail.com",
    password="your_password", 
    chapter_title="Bab 28: Pertarungan Terakhir",
    chapter_content="Isi chapter yang panjang minimal 1000 karakter..."
)
```

## ⚙️ Workflow Automation

### Mendapatkan Daftar Novel

Automation ini mengikuti workflow manual user:

1. **🌐 Buka fizzo.org**
2. **📱 Klik hamburger menu (☰)**
3. **✍️ Klik "Menulis Cerita"**
4. **📧 Klik "Lanjutkan dengan Email"**
5. **📝 Fill email field**
6. **🔒 Fill password field**
7. **🚀 Klik "Lanjut" button**
8. **⏳ Wait for dashboard**
9. **🔍 Scan halaman untuk daftar novel**
10. **📋 Ekstrak judul dan ID novel**

### Upload Chapter Novel

Automation ini mengikuti workflow manual user:

1. **🌐 Buka fizzo.org**
2. **📱 Klik hamburger menu (☰)**
3. **✍️ Klik "Menulis Cerita"**
4. **📧 Klik "Lanjutkan dengan Email"**
5. **📝 Fill email field**
6. **🔒 Fill password field**
7. **🚀 Klik "Lanjut" button**
8. **⏳ Wait for dashboard**
9. **📚 Pilih novel berdasarkan ID (jika diberikan)**
10. **📝 Klik "New Chapter" button**
11. **📖 Fill chapter title**
12. **📄 Fill chapter content**
13. **✈️ Klik publish button**

## 🛡️ Security & Validation

### Input Validation
- ✅ Email dan password required
- ✅ Chapter title dan content required
- ✅ Content minimal 1,000 karakter
- ✅ Content maksimal 60,000 karakter

### Security Features
- ✅ Headless browser (tidak tampil UI)
- ✅ Auto-close browser setelah selesai
- ✅ Error handling untuk timeout
- ✅ No credential logging

### Browser Security
- ✅ No-sandbox mode untuk container
- ✅ Disable GPU untuk stability
- ✅ Mobile user agent
- ✅ Network idle wait

## 🚨 Error Handling

### Common Errors

| Error | Cause | Solution |
|-------|-------|----------|
| `Login failed` | Email/password salah | Cek credentials |
| `Chapter content must be at least 1,000 characters` | Content terlalu pendek | Tambah content |
| `Chapter content must be less than 60,000 characters` | Content terlalu panjang | Potong content |
| `Could not find chapter content field` | Selector tidak ditemukan | Coba lagi atau report bug |
| `Browser timeout` | Network lambat | Coba lagi |

### Debugging

Untuk debugging, cek logs di HF Spaces:

```
🚀 Starting Fizzo auto-update for chapter: Bab 28
🌐 Navigating to fizzo.org...
📱 Clicking hamburger menu...
✍️ Clicking 'Menulis Cerita'...
📧 Clicking 'Lanjutkan dengan Email'...
📝 Filling email field...
🔒 Filling password field...
🚀 Clicking 'Lanjut' button...
⏳ Waiting for dashboard...
✅ Login successful - Dashboard loaded
📝 Clicking 'New Chapter' button...
📖 Filling chapter title: Bab 28
📄 Filling chapter content (2500 characters)...
💾 Waiting for auto-save...
🚀 Publishing chapter...
✅ Chapter creation completed
```

## 🔧 Deployment Notes

### HF Spaces Requirements
- ✅ Playwright akan auto-install Chromium browser
- ✅ Headless mode untuk container environment
- ✅ Memory optimized untuk HF Spaces limits
- ✅ Timeout handling untuk HF Spaces restrictions

### Performance
- ⏱️ **Login**: ~10-15 detik
- ⏱️ **Chapter Upload**: ~5-10 detik
- ⏱️ **Total**: ~15-25 detik per chapter
- 💾 **Memory**: ~200-300MB saat browser aktif

## 🎯 Integration dengan Frontend

### Vercel Frontend Example

```javascript
// components/FizzoUploader.js
import { useState } from 'react';

export default function FizzoUploader() {
  const [loading, setLoading] = useState(false);
  const [result, setResult] = useState(null);
  
  const uploadChapter = async (formData) => {
    setLoading(true);
    try {
      const response = await fetch(`${process.env.NEXT_PUBLIC_BACKEND_URL}/api/fizzo-auto-update`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
      });
      
      const result = await response.json();
      setResult(result);
    } catch (error) {
      setResult({ success: false, error: error.message });
    } finally {
      setLoading(false);
    }
  };
  
  return (
    <div>
      {/* Form UI here */}
      {loading && <p>🚀 Uploading chapter...</p>}
      {result && (
        <div>
          {result.success ? 
            <p>✅ Chapter uploaded successfully!</p> : 
            <p>❌ Error: {result.error}</p>
          }
        </div>
      )}
    </div>
  );
}
```

## 📝 Changelog

### v1.1.0
- ✅ Tambah fitur scraping daftar novel user
- ✅ Tambah endpoint `/api/fizzo-list-novel`
- ✅ Support pilih novel berdasarkan ID
- ✅ Update endpoint `/api/fizzo-auto-update` dengan parameter `novel_id`
- ✅ Dokumentasi lengkap untuk fitur baru

### v1.0.0
- ✅ Initial implementation
- ✅ Complete automation workflow
- ✅ Error handling dan validation
- ✅ HF Spaces optimization
- ✅ Security features

---

**🎉 Selamat! Fitur Fizzo Auto-Update siap digunakan!**

Untuk pertanyaan atau bug report, silakan buat issue di repository ini.