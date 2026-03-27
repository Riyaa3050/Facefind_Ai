# 🔍 FaceFind — AI-Powered Smart Photo Discovery

> Find yourself in thousands of event photos in seconds using **Face Recognition** + **Scene Understanding**.

Built with **Streamlit** · **DuckDB** · **CLIP** · **YOLOv8** · **DeepFace (ArcFace)** · **FAISS**

---

## 🚀 Quick Start

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

> ⚠️ **First run downloads AI models (~1.5 GB):** CLIP ViT-B/32, YOLOv8n, ArcFace. Be patient on first launch.

### 2. Run the App

```bash
streamlit run app.py
```

Open → **http://localhost:8501**

---

## 🏗️ Architecture

```
Admin uploads Google Drive link
       ↓
Photos downloaded via gdown
       ↓
┌─────────────────────┐
│  Scene Engine       │  CLIP (semantic tagging) + YOLOv8 (object detection)
│  → scene_label      │
└─────────────────────┘
       ↓
┌─────────────────────┐
│  Face Engine        │  DeepFace ArcFace embeddings + FAISS index
│  → 512-d vectors    │
└─────────────────────┘
       ↓
┌─────────────────────┐
│  DuckDB             │  photos, users, face_matches, search_logs
└─────────────────────┘
       ↓
User uploads selfie → FAISS search → matched photos displayed
```

---

## 🎯 Features

### 🛠️ Admin Dashboard
| Feature | Description |
|---|---|
| Google Drive Upload | Paste any public Drive folder URL |
| Scene Understanding | Auto-classifies photos into 10 scene types |
| Face Embeddings | Extracts & indexes faces with ArcFace |
| Progress Tracking | Live per-photo progress with scene breakdown chart |
| Event Manager | View, browse, and delete events |
| Analytics | Platform-wide stats (photos, users, searches, matches) |

### 🧑‍💻 User Dashboard
| Feature | Description |
|---|---|
| Register / Login | Secure bcrypt password hashing |
| Selfie Search | Upload photo → find your matches across all events |
| Scene Filter | Narrow search to specific scene types |
| Event Filter | Search within a specific event |
| Match Sensitivity | Adjustable threshold slider |
| My Library | All your previously matched photos |
| Download | Per-photo download button |
| Browse by Scene | Explore all event photos organized by scene |

---

## 📁 Project Structure

```
facefind/
├── app.py                    # Streamlit entry point
├── requirements.txt
├── .env.example
├── database/
│   └── db.py                 # DuckDB schema + CRUD
├── services/
│   ├── scene_engine.py       # CLIP + YOLOv8
│   ├── face_engine.py        # DeepFace + FAISS
│   └── drive_utils.py        # Google Drive download
├── pages/
│   ├── admin_dashboard.py    # Admin UI
│   └── user_dashboard.py     # User UI
├── data/
│   ├── facefind.duckdb       # Auto-created
│   ├── faiss_index.bin       # Auto-created
│   └── uploads/              # Downloaded event images
└── .streamlit/
    └── config.toml           # Dark theme
```

---

## 🔐 Default Admin Credentials

```
Email:    admin@facefind.ai
Password: admin123
```

---

## 🎭 Scene Categories

| Scene | Emoji | CLIP Prompt |
|---|---|---|
| Stage Performance | 🎤 | person on stage with microphone and lights |
| Award Ceremony | 🏆 | trophy presentation and formal dress |
| Group Photo | 👥 | large group posing together |
| Dining Event | 🍽️ | people eating at social event |
| Outdoor Event | 🌳 | people gathered outdoors |
| Sports Event | ⚽ | athletic activities |
| Seminar Talk | 🎓 | listening to speaker/presentation |
| Candid Moment | 📸 | natural informal conversations |
| Cultural Event | 🎭 | dance/music/cultural performance |
| Entrance Lobby | 🚪 | near entrance/registration desk |

---

## ⚙️ Environment Variables

Copy `.env.example` → `.env` and customize:

```env
ADMIN_EMAIL=admin@facefind.ai
ADMIN_PASSWORD=admin123
DB_PATH=data/facefind.duckdb
FAISS_INDEX_PATH=data/faiss_index.bin
UPLOAD_DIR=data/uploads
```

---

## 📝 Judge Demo Script

1. **Admin tab** → Login → Upload Drive link → Watch AI pipeline process photos
2. **User tab** → Register → Upload selfie → Click "Find My Photos"
3. Show scene filter: select "Award Ceremony" → search → see filtered results
4. Download matched photos

---

*Built for Nirma Hackathon 2025 · Team: Code Titans*
