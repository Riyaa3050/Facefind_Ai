# 🏆 FaceFind — Hackathon Build Guide
**Team: Code Titans | Track: Software — Data Science & ML**
**Duration: 36 Hours | Goal: Win Every Challenge**

---

## 📌 Table of Contents
1. [Project Overview](#project-overview)
2. [Architecture Blueprint](#architecture-blueprint)
3. [Tech Stack](#tech-stack)
4. [Folder Structure](#folder-structure)
5. [Phase-wise Build Plan (36 Hours)](#phase-wise-build-plan)
6. [Module 1 — Authentication System](#module-1--authentication-system)
7. [Module 2 — Admin Photo Upload & Storage](#module-2--admin-photo-upload--storage)
8. [Module 3 — Scene Understanding Engine](#module-3--scene-understanding-engine)
9. [Module 4 — Face Recognition Engine](#module-4--face-recognition-engine)
10. [Module 5 — Smart Search & Retrieval](#module-5--smart-search--retrieval)
11. [Module 6 — Frontend UI/UX](#module-6--frontend-uiux)
12. [Module 7 — API Layer](#module-7--api-layer)
13. [Database Schema](#database-schema)
14. [Environment Setup](#environment-setup)
15. [Demo Script (for Judges)](#demo-script-for-judges)
16. [Key Differentiators to Highlight](#key-differentiators-to-highlight)
17. [Potential Judge Questions & Answers](#potential-judge-questions--answers)

---

## Project Overview

**FaceFind** is an AI-powered Smart Photo Discovery Platform that solves a real-world pain point: finding your own photos from thousands of event images. The platform uniquely combines two AI engines:

| Engine | What it Does |
|---|---|
| **Face Recognition** | Identifies a person across photos using facial embeddings |
| **Scene Understanding** | Analyzes and categorizes photos by context (stage, crowd, dining, outdoor, etc.) |

**The USP that no competitor has:** Scene Understanding. Before even scanning faces, FaceFind groups photos by *where* and *what* is happening — creating intelligent folders that mirror real-world events. Face search is then performed **within context**, making it dramatically faster and more accurate.

---

## Architecture Blueprint

```
┌─────────────────────────────────────────────────────────┐
│                     FACEFIND PLATFORM                   │
├─────────────────┬───────────────────────────────────────┤
│   ADMIN SIDE    │              USER SIDE                │
│                 │                                       │
│  Upload Bulk    │   Register/Login → Upload Selfie      │
│  Event Photos   │          ↓                            │
│       ↓         │   Scene Filter (optional)             │
│  Scene Engine   │          ↓                            │
│  (YOLO/CLIP)    │   Face Match Engine                   │
│       ↓         │          ↓                            │
│  Auto-folder    │   View & Download My Photos           │
│  by Scene       │          ↓                            │
│       ↓         │   Privacy-controlled Access           │
│  Face Embed     │                                       │
│  (DeepFace)     │                                       │
│       ↓         │                                       │
│  Store in DB    │                                       │
└─────────────────┴───────────────────────────────────────┘
          ↕                    ↕
    [ Firebase / Supabase Storage ]
    [ PostgreSQL / Firestore DB   ]
    [ FastAPI Backend             ]
    [ React Frontend              ]
```

---

## Tech Stack

### Backend
| Layer | Technology | Reason |
|---|---|---|
| API Framework | **FastAPI** (Python) | Async, fast, auto-docs |
| Face Recognition | **DeepFace** + **face_recognition** | Accurate embeddings, easy API |
| Scene Understanding | **CLIP (OpenAI)** + **YOLOv8** | CLIP for semantic scene tagging, YOLO for object detection |
| Image Processing | **OpenCV**, **Pillow** | Preprocessing pipeline |
| Vector Search | **FAISS** | Fast similarity search for face embeddings |
| Database | **PostgreSQL** (via Supabase) or **Firebase Firestore** | Auth + DB in one |
| Storage | **Firebase Storage** or **Supabase Storage** | S3-compatible, free tier |
| Authentication | **Firebase Auth** or **Supabase Auth** | JWT, OAuth, Email/Pass |

### Frontend
| Layer | Technology |
|---|---|
| Framework | **React.js** + Vite |
| UI Library | **TailwindCSS** + **ShadCN UI** |
| Camera/Selfie | **react-webcam** |
| State | **Zustand** or Context API |
| API calls | **Axios** |

### DevOps (for demo)
- **Docker Compose** — containerize backend + DB locally
- **ngrok** — expose local server for demo
- **Render / Railway** — optional free deployment

---

## Folder Structure

```
facefind/
├── backend/
│   ├── main.py                  # FastAPI entry point
│   ├── routers/
│   │   ├── auth.py              # Login, Register, JWT
│   │   ├── upload.py            # Admin bulk photo upload
│   │   ├── search.py            # Face search endpoint
│   │   └── scene.py             # Scene categorization
│   ├── services/
│   │   ├── face_engine.py       # DeepFace embedding + FAISS search
│   │   ├── scene_engine.py      # CLIP + YOLO scene analysis
│   │   ├── storage.py           # Firebase/Supabase storage handlers
│   │   └── embedding_store.py   # Vector DB management
│   ├── models/
│   │   ├── user.py
│   │   ├── photo.py
│   │   └── scene.py
│   ├── database.py              # DB connection
│   ├── config.py                # Env vars
│   └── requirements.txt
│
├── frontend/
│   ├── src/
│   │   ├── pages/
│   │   │   ├── Login.jsx
│   │   │   ├── Register.jsx
│   │   │   ├── Dashboard.jsx    # Scene folders + photos
│   │   │   ├── SelfieSearch.jsx # Camera capture + results
│   │   │   └── AdminUpload.jsx  # Bulk upload page
│   │   ├── components/
│   │   │   ├── SceneCard.jsx    # Folder card with scene label
│   │   │   ├── PhotoGrid.jsx    # Results grid
│   │   │   ├── WebcamCapture.jsx
│   │   │   └── Navbar.jsx
│   │   ├── services/
│   │   │   └── api.js           # Axios API calls
│   │   └── App.jsx
│   └── package.json
│
├── docker-compose.yml
├── .env.example
└── README.md
```

---

## Phase-wise Build Plan

### ⏰ Hour 0–2 | Setup & Scaffolding
- [ ] Create GitHub repo, add all team members
- [ ] Setup Python virtual env, install all dependencies
- [ ] Initialize React app with Vite + TailwindCSS
- [ ] Setup Firebase project (Auth + Storage + Firestore) **OR** Supabase
- [ ] Create `.env` files with API keys
- [ ] Run `uvicorn main:app --reload` — confirm FastAPI is live
- [ ] Confirm React app runs at `localhost:5173`

### ⏰ Hour 2–6 | Authentication + Storage Layer
- [ ] Implement Register/Login API (FastAPI + Firebase Auth)
- [ ] JWT middleware for protected routes
- [ ] Admin vs User role separation
- [ ] Storage bucket setup with folder structure: `events/{event_id}/photos/`
- [ ] Frontend: Login page, Register page, protected routing

### ⏰ Hour 6–12 | Scene Understanding Engine ⭐ (USP)
- [ ] Integrate CLIP model for semantic scene tagging
- [ ] Define scene categories: `stage`, `crowd`, `ceremony`, `dining`, `outdoor`, `indoor`, `sports`, `award`, `group_photo`, `candid`
- [ ] Integrate YOLOv8 for object detection within scenes
- [ ] Build `scene_engine.py` — takes image → returns scene label + detected objects
- [ ] Auto-folder creation logic: group photos by scene
- [ ] Store scene metadata in DB per photo
- [ ] Test with sample event photos

### ⏰ Hour 12–18 | Face Recognition Engine
- [ ] Build face embedding pipeline using DeepFace (model: `ArcFace` for best accuracy)
- [ ] FAISS index setup for fast vector similarity search
- [ ] On admin upload: extract face embeddings → store in FAISS + DB
- [ ] On user selfie: extract embedding → search FAISS index → return top matches
- [ ] Confidence threshold filtering (e.g., distance < 0.4 = match)
- [ ] Link matched photos to user's account

### ⏰ Hour 18–24 | API Integration + Frontend Core
- [ ] `/api/upload` — Admin bulk photo upload endpoint
- [ ] `/api/search` — User selfie → matched photos
- [ ] `/api/scenes` — Get all scene categories with photo counts
- [ ] `/api/photos/{scene}` — Get photos by scene
- [ ] Frontend: Dashboard with scene folders (like Google Photos albums)
- [ ] Frontend: Selfie capture page with webcam + upload
- [ ] Frontend: Photo results grid with download option

### ⏰ Hour 24–30 | Polish + Integration
- [ ] Connect all frontend pages to real backend APIs
- [ ] Loading states, error handling, toast notifications
- [ ] Scene Understanding visual: show detected objects on hover
- [ ] Add search filter: "Search by face within this scene folder"
- [ ] Admin upload progress bar for bulk photos
- [ ] Optimize FAISS search for demo dataset

### ⏰ Hour 30–34 | Testing + Bug Fixes
- [ ] Full end-to-end test with real photos
- [ ] Test with multiple users to ensure privacy (user A cannot see user B's results)
- [ ] Test scene categorization accuracy
- [ ] Fix UI bugs, ensure mobile responsiveness
- [ ] Prepare demo dataset (30–50 photos from a mock event)

### ⏰ Hour 34–36 | Demo Prep
- [ ] Record a 2-minute demo video (backup)
- [ ] Prepare judge walkthrough script
- [ ] Ensure app runs smoothly on demo machine
- [ ] Deploy or run via ngrok for live demo
- [ ] Prepare pitch: problem → solution → demo → impact → future scope

---

## Module 1 — Authentication System

### Backend (`routers/auth.py`)
```python
from fastapi import APIRouter, HTTPException, Depends
from services.firebase_auth import verify_token, create_user
from models.user import UserRegister, UserLogin
import jwt, os

router = APIRouter(prefix="/auth", tags=["Authentication"])

@router.post("/register")
async def register(user: UserRegister):
    """Register new user with email + password"""
    try:
        firebase_user = create_user(user.email, user.password, user.name)
        return {"uid": firebase_user.uid, "message": "User created successfully"}
    except Exception as e:
        raise HTTPException(status_code=400, detail=str(e))

@router.post("/login")
async def login(credentials: UserLogin):
    """Login and return JWT token"""
    # Firebase handles auth, return custom token
    token = verify_token(credentials.id_token)
    return {"access_token": token, "token_type": "bearer"}

@router.get("/me")
async def get_current_user(token: str = Depends(oauth2_scheme)):
    """Get current authenticated user"""
    payload = jwt.decode(token, os.getenv("JWT_SECRET"), algorithms=["HS256"])
    return {"uid": payload["uid"], "email": payload["email"], "role": payload["role"]}
```

### Key Points
- Two roles: `admin` (uploads photos) and `user` (searches photos)
- JWT stored in `localStorage` on frontend
- All photo retrieval endpoints require valid JWT
- User can only download **their own** matched photos

---

## Module 2 — Admin Photo Upload & Storage

### Backend (`routers/upload.py`)
```python
@router.post("/upload/bulk")
async def bulk_upload(
    event_name: str,
    files: List[UploadFile] = File(...),
    current_user: dict = Depends(require_admin)
):
    results = []
    for file in files:
        # 1. Upload to Firebase Storage
        image_url = await storage.upload(file, f"events/{event_name}/{file.filename}")
        
        # 2. Run Scene Understanding
        scene_data = scene_engine.analyze(image_bytes)
        # Returns: {"scene": "stage", "objects": ["person", "microphone", "podium"], "confidence": 0.92}
        
        # 3. Extract Face Embeddings
        embeddings = face_engine.extract_embeddings(image_bytes)
        # Returns list of face embeddings found in the photo
        
        # 4. Store everything in DB
        photo_record = await db.photos.insert({
            "url": image_url,
            "event": event_name,
            "scene_label": scene_data["scene"],
            "detected_objects": scene_data["objects"],
            "face_count": len(embeddings),
            "uploaded_by": current_user["uid"]
        })
        
        # 5. Store embeddings in FAISS with photo_id reference
        face_engine.add_to_index(embeddings, photo_record.id)
        
        results.append({"file": file.filename, "scene": scene_data["scene"]})
    
    return {"uploaded": len(results), "results": results}
```

---

## Module 3 — Scene Understanding Engine

### The Core Differentiator ⭐

```python
# services/scene_engine.py
import clip
import torch
from PIL import Image
from ultralytics import YOLO
import io

class SceneEngine:
    # Scene categories with descriptive prompts for CLIP
    SCENE_CATEGORIES = {
        "stage_performance": "a person performing on stage with microphone and lights",
        "award_ceremony": "an award ceremony with trophy and formal dress",
        "group_photo": "a large group of people posing together for a photo",
        "dining_event": "people sitting at tables eating food at a social event",
        "outdoor_event": "people gathered outdoors in an open area or field",
        "sports_event": "people playing sports or athletic activities",
        "seminar_talk": "people sitting in rows listening to a speaker or presentation",
        "candid_moment": "people having natural candid conversations",
        "cultural_event": "traditional dance, music, or cultural performance",
        "entrance_lobby": "people near entrance, gate, or registration desk"
    }
    
    def __init__(self):
        self.clip_model, self.clip_preprocess = clip.load("ViT-B/32")
        self.yolo_model = YOLO("yolov8n.pt")  # nano model for speed
        self.text_features = self._encode_scene_texts()
    
    def _encode_scene_texts(self):
        texts = list(self.SCENE_CATEGORIES.values())
        tokens = clip.tokenize(texts)
        with torch.no_grad():
            return self.clip_model.encode_text(tokens)
    
    def analyze(self, image_bytes: bytes) -> dict:
        image = Image.open(io.BytesIO(image_bytes)).convert("RGB")
        
        # CLIP: Scene Classification
        preprocessed = self.clip_preprocess(image).unsqueeze(0)
        with torch.no_grad():
            image_features = self.clip_model.encode_image(preprocessed)
        
        similarity = (image_features @ self.text_features.T).softmax(dim=-1)
        scene_idx = similarity.argmax().item()
        scene_label = list(self.SCENE_CATEGORIES.keys())[scene_idx]
        confidence = similarity[0][scene_idx].item()
        
        # YOLO: Object Detection
        yolo_results = self.yolo_model(image, verbose=False)
        detected_objects = list(set([
            yolo_results[0].names[int(cls)] 
            for cls in yolo_results[0].boxes.cls
        ]))
        
        return {
            "scene": scene_label,
            "confidence": round(confidence, 3),
            "objects": detected_objects,
            "object_count": len(yolo_results[0].boxes)
        }

scene_engine = SceneEngine()
```

### Scene → Folder Mapping (Frontend)
```
📁 Stage Performance (12 photos)
📁 Group Photos (34 photos)
📁 Award Ceremony (8 photos)
📁 Dining Event (19 photos)
📁 Seminar Talk (27 photos)
📁 Candid Moments (41 photos)
```
Users can browse by scene OR scan their face within a specific scene folder.

---

## Module 4 — Face Recognition Engine

```python
# services/face_engine.py
from deepface import DeepFace
import faiss
import numpy as np
import pickle, os

class FaceEngine:
    EMBEDDING_DIM = 512  # ArcFace embedding size
    DISTANCE_THRESHOLD = 0.4  # Lower = stricter match
    
    def __init__(self):
        self.index = faiss.IndexFlatL2(self.EMBEDDING_DIM)
        self.photo_id_map = []  # Maps FAISS index position → photo_id
        self._load_existing_index()
    
    def extract_embeddings(self, image_bytes: bytes) -> list:
        """Extract face embeddings from an image (may contain multiple faces)"""
        try:
            result = DeepFace.represent(
                img_path=image_bytes,
                model_name="ArcFace",
                detector_backend="retinaface",  # Best detector
                enforce_detection=False
            )
            return [face["embedding"] for face in result]
        except Exception:
            return []
    
    def add_to_index(self, embeddings: list, photo_id: str):
        """Add face embeddings to FAISS search index"""
        for embedding in embeddings:
            vec = np.array([embedding], dtype=np.float32)
            self.index.add(vec)
            self.photo_id_map.append(photo_id)
        self._save_index()
    
    def search(self, selfie_bytes: bytes, scene_filter: str = None, top_k: int = 50) -> list:
        """Search for matching photos given a selfie"""
        embeddings = self.extract_embeddings(selfie_bytes)
        if not embeddings:
            return []
        
        query_vec = np.array([embeddings[0]], dtype=np.float32)
        distances, indices = self.index.search(query_vec, top_k)
        
        matched_photo_ids = []
        for dist, idx in zip(distances[0], indices[0]):
            if idx != -1 and dist < self.DISTANCE_THRESHOLD:
                matched_photo_ids.append(self.photo_id_map[idx])
        
        return list(set(matched_photo_ids))  # Deduplicate
    
    def _save_index(self):
        faiss.write_index(self.index, "face_index.bin")
        with open("photo_id_map.pkl", "wb") as f:
            pickle.dump(self.photo_id_map, f)
    
    def _load_existing_index(self):
        if os.path.exists("face_index.bin"):
            self.index = faiss.read_index("face_index.bin")
            with open("photo_id_map.pkl", "rb") as f:
                self.photo_id_map = pickle.load(f)

face_engine = FaceEngine()
```

---

## Module 5 — Smart Search & Retrieval

```python
# routers/search.py
@router.post("/search/face")
async def search_by_face(
    selfie: UploadFile = File(...),
    scene_filter: Optional[str] = None,  # Optional: search within a scene
    current_user: dict = Depends(get_current_user)
):
    """Main face search endpoint"""
    image_bytes = await selfie.read()
    
    # Get matching photo IDs from FAISS
    matched_ids = face_engine.search(image_bytes, scene_filter)
    
    if not matched_ids:
        return {"found": 0, "photos": []}
    
    # Fetch photo metadata from DB
    query = db.photos.where("id", "in", matched_ids)
    if scene_filter:
        query = query.where("scene_label", "==", scene_filter)
    
    photos = await query.get()
    
    # Log search for analytics
    await db.search_logs.insert({
        "user_id": current_user["uid"],
        "results_count": len(photos),
        "scene_filter": scene_filter,
        "timestamp": datetime.now()
    })
    
    return {
        "found": len(photos),
        "photos": [{"id": p.id, "url": p.url, "scene": p.scene_label, "event": p.event} for p in photos]
    }
```

---

## Module 6 — Frontend UI/UX

### Key Pages

#### `SelfieSearch.jsx` — The Hero Feature
```jsx
import Webcam from "react-webcam";
import { useState, useRef } from "react";
import { searchByFace } from "../services/api";

export default function SelfieSearch() {
  const webcamRef = useRef(null);
  const [results, setResults] = useState([]);
  const [loading, setLoading] = useState(false);
  const [selectedScene, setSelectedScene] = useState(null);

  const captureAndSearch = async () => {
    setLoading(true);
    const imageSrc = webcamRef.current.getScreenshot();
    const blob = dataURLtoBlob(imageSrc);
    const formData = new FormData();
    formData.append("selfie", blob, "selfie.jpg");
    if (selectedScene) formData.append("scene_filter", selectedScene);

    const data = await searchByFace(formData);
    setResults(data.photos);
    setLoading(false);
  };

  return (
    <div className="flex flex-col items-center gap-6 p-8">
      <h1 className="text-3xl font-bold">Find My Photos</h1>
      
      {/* Optional Scene Filter */}
      <div className="flex gap-2 flex-wrap justify-center">
        {SCENES.map(scene => (
          <button
            key={scene}
            onClick={() => setSelectedScene(selectedScene === scene ? null : scene)}
            className={`px-3 py-1 rounded-full text-sm border ${
              selectedScene === scene ? "bg-blue-600 text-white" : "bg-white"
            }`}
          >
            {scene.replace("_", " ")}
          </button>
        ))}
      </div>

      {/* Webcam */}
      <Webcam
        ref={webcamRef}
        screenshotFormat="image/jpeg"
        className="rounded-xl shadow-lg w-80 h-60 object-cover"
      />
      
      <button
        onClick={captureAndSearch}
        disabled={loading}
        className="bg-blue-600 text-white px-8 py-3 rounded-xl text-lg font-semibold hover:bg-blue-700"
      >
        {loading ? "🔍 Scanning..." : "📸 Scan & Find Photos"}
      </button>

      {/* Results */}
      {results.length > 0 && (
        <div>
          <p className="text-green-600 font-bold text-center">✅ Found {results.length} photos!</p>
          <div className="grid grid-cols-3 gap-3 mt-4">
            {results.map(photo => (
              <div key={photo.id} className="relative group">
                <img src={photo.url} className="rounded-lg w-full h-32 object-cover" />
                <div className="absolute inset-0 bg-black/50 opacity-0 group-hover:opacity-100 flex items-center justify-center rounded-lg">
                  <a href={photo.url} download className="text-white text-sm">⬇ Download</a>
                </div>
                <span className="text-xs text-gray-500 mt-1 block">{photo.scene}</span>
              </div>
            ))}
          </div>
        </div>
      )}
    </div>
  );
}
```

#### `Dashboard.jsx` — Scene Folder View
```jsx
// Scene folders like Google Photos albums
// Click a folder → see photos in that scene
// Click "Find Me Here" → run face search within that scene only
```

---

## Module 7 — API Layer

### All Endpoints Summary

| Method | Endpoint | Auth | Description |
|---|---|---|---|
| POST | `/auth/register` | ❌ | Register new user |
| POST | `/auth/login` | ❌ | Login, get JWT |
| GET | `/auth/me` | ✅ | Get current user |
| POST | `/upload/bulk` | ✅ Admin | Upload bulk photos for event |
| GET | `/scenes` | ✅ | Get all scene folders with counts |
| GET | `/scenes/{scene}/photos` | ✅ | Get photos in a scene |
| POST | `/search/face` | ✅ | Search by selfie (+ optional scene filter) |
| GET | `/photos/my` | ✅ | Get all photos matched to current user |
| GET | `/events` | ✅ Admin | Get all events |
| DELETE | `/photos/{id}` | ✅ Admin | Delete a photo |

---

## Database Schema

### Tables / Collections

#### `users`
```sql
id          UUID PRIMARY KEY
email       VARCHAR UNIQUE NOT NULL
name        VARCHAR
role        ENUM('admin', 'user') DEFAULT 'user'
created_at  TIMESTAMP
```

#### `photos`
```sql
id              UUID PRIMARY KEY
url             VARCHAR NOT NULL
event_name      VARCHAR
scene_label     VARCHAR       -- from Scene Engine
detected_objects JSONB        -- ["person", "microphone", "podium"]
scene_confidence FLOAT
face_count      INTEGER
uploaded_by     UUID REFERENCES users(id)
created_at      TIMESTAMP
```

#### `face_matches`
```sql
id          UUID PRIMARY KEY
user_id     UUID REFERENCES users(id)
photo_id    UUID REFERENCES photos(id)
confidence  FLOAT
matched_at  TIMESTAMP
```

#### `search_logs`
```sql
id              UUID PRIMARY KEY
user_id         UUID
results_count   INTEGER
scene_filter    VARCHAR
searched_at     TIMESTAMP
```

---

## Environment Setup

### `requirements.txt`
```
fastapi==0.111.0
uvicorn==0.30.0
deepface==0.0.92
faiss-cpu==1.8.0
clip @ git+https://github.com/openai/CLIP.git
ultralytics==8.2.0
opencv-python-headless==4.9.0.80
Pillow==10.3.0
torch==2.3.0
torchvision==0.18.0
firebase-admin==6.5.0
python-multipart==0.0.9
python-jose==3.3.0
passlib==1.7.4
SQLAlchemy==2.0.30
psycopg2-binary==2.9.9
numpy==1.26.4
python-dotenv==1.0.1
```

### `.env.example`
```env
# Firebase
FIREBASE_PROJECT_ID=your-project-id
FIREBASE_PRIVATE_KEY=your-private-key
FIREBASE_CLIENT_EMAIL=your-client-email
FIREBASE_STORAGE_BUCKET=your-bucket.appspot.com

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/facefind

# JWT
JWT_SECRET=your-super-secret-key-here
JWT_EXPIRE_HOURS=24

# App
ENVIRONMENT=development
```

### Quick Start Commands
```bash
# Backend
cd backend
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload --port 8000

# Frontend
cd frontend
npm install
npm run dev

# Visit: http://localhost:5173
# API Docs: http://localhost:8000/docs
```

---

## Demo Script (for Judges)

### 🎯 Opening (30 seconds)
> "Imagine you attended a college fest with 3,000 photos taken. You want to find YOUR photos. Today, you manually scroll through thousands of images. FaceFind solves this in under 5 seconds — but we've added something that nobody else has: **Scene Understanding**."

### 🎬 Demo Flow (3–4 minutes)

**Step 1 — Admin uploads photos**
> "As an event organizer, I upload 50 photos from today's event. Watch how FaceFind automatically organizes them into intelligent folders — Stage Performance, Group Photos, Award Ceremony, Candid Moments — using AI scene analysis."
*(Show the auto-generated scene folders)*

**Step 2 — Scene Understanding visualization**
> "Each photo isn't just stored — it's understood. This photo is classified as 'Stage Performance' with 94% confidence, and the system detected: person, microphone, podium, lights. This is real computer vision, not just metadata."
*(Hover over a photo to show detected objects)*

**Step 3 — Face Search (the magic)**
> "Now as a user, I want to find MY photos. I simply scan my face using the webcam..."
*(Capture selfie → click search)*
> "In under 3 seconds, FaceFind found [X] photos of me across all scenes."

**Step 4 — Scene-specific search**
> "But here's the power — I can also say: find me ONLY in the Award Ceremony photos."
*(Select scene filter → scan again)*
> "This makes it incredibly precise for large events where thousands of photos exist."

**Step 5 — Privacy & Security**
> "Every result is tied to a user account. I can only see MY matched photos. Nobody else can access them."

### 💡 Closing (30 seconds)
> "FaceFind isn't just face recognition — it's the first platform to combine face recognition WITH scene understanding. You don't just find yourself, you find yourself **in context**. This has direct applications for universities, corporate events, weddings, sports events, and public gatherings. The market opportunity is massive, and we've built a working MVP in 36 hours."

---

## Key Differentiators to Highlight

| What exists today | What FaceFind adds |
|---|---|
| Face recognition (Smilebox, Memories) | ✅ Already covered |
| Manual album organization | ✅ Replaced with Scene AI |
| Search by person | ✅ Already covered |
| **Search by person + scene context** | 🚀 **FaceFind's USP — unique** |
| Photo storage | ✅ Already covered |
| **Object detection within scenes** | 🚀 **Shows what's happening in each photo** |
| Basic privacy | ✅ Already covered |
| **Role-based multi-event access** | 🚀 **Admin/User separation** |

---

## Potential Judge Questions & Answers

**Q: How is this different from Google Photos?**
> Google Photos does face grouping for personal albums. FaceFind is built for large-scale events where you don't own the photos — you need to retrieve yours from a shared dataset of thousands. Plus, scene understanding categorizes by event context, not just by person.

**Q: What's the accuracy of face recognition?**
> We use ArcFace model via DeepFace, which achieves 99.4% accuracy on LFW benchmark. With our 0.4 distance threshold and RetinaFace detector, we maintain high precision. For low-quality images, we handle graceful fallback with a "no match found" message.

**Q: How does Scene Understanding work technically?**
> We use OpenAI's CLIP model — a vision-language model trained on 400M image-text pairs. We define scene categories as natural language prompts and use cosine similarity to classify each image. YOLO v8 runs in parallel to detect specific objects. Both run on upload, not at search time, so retrieval is instant.

**Q: What about privacy and GDPR?**
> Users must authenticate to access any photos. Face embeddings are stored as mathematical vectors, not as raw biometric data. Users can request deletion of their matched data. Admins control event access.

**Q: Can it scale?**
> FAISS (Facebook AI Similarity Search) can handle billions of vectors with millisecond query times. Firebase Storage and Supabase are infinitely scalable. The scene engine processes asynchronously using FastAPI's background tasks.

**Q: What's the business model?**
> B2B SaaS: charge event organizers per event or per photo. Universities, corporate HR for events, wedding photographers, and sports academies are primary customers. Freemium: small events free, large events paid.

---

## 🏁 Final Checklist Before Demo

- [ ] Test with 3 different users' faces — all return correct photos only
- [ ] Scene folders show correct categories
- [ ] Object detection labels visible on hover
- [ ] Admin bulk upload processes 20+ photos without crash
- [ ] Download button works for matched photos
- [ ] Login/Logout flow works
- [ ] App is mobile-responsive (for judge on phone)
- [ ] API docs available at `/docs` for technical judges
- [ ] Demo dataset ready (variety of scenes, multiple people)
- [ ] Backup: recorded demo video as fallback

---

*Built by Code Titans — Riya Navadia & Disha Vekaria*
*"Don't just find faces. Understand the scene."*
