# 🔥 FireVision Pro
### AI-Powered Real-Time Fire and Smoke Detection System

FireVision Pro is an AI-powered fire and smoke detection platform designed to monitor CCTV camera feeds in real time. It uses a YOLOv8-based deep learning model with ONNX Runtime to identify fire and smoke from video streams and generate alerts when potential fire incidents are detected.

The system provides a desktop monitoring interface, centralized backend services, alert management, and a geographical map interface to visualize camera locations and detected fire incidents.

## 1. Overview

FireVision Pro provides the following functionalities:

- **Real-Time Fire Detection:** Detects fire and smoke from live CCTV camera streams.
- **AI-Based Detection:** Uses YOLOv8 for object detection and ONNX Runtime for model inference.
- **Live Camera Monitoring:** Displays camera feeds through a PyQt5 desktop application.
- **Alert Management:** Generates and manages alerts when fire or smoke is detected.
- **Camera Management:** Allows users to register, configure, and monitor cameras.
- **Location Mapping:** Displays camera locations and detected fire incidents using Leaflet maps.
- **User Authentication:** Provides secure login and role-based access.
- **Centralized Database:** Stores camera details, user information, and fire detection alerts.

## 2. System Architecture

```text
             CCTV CAMERA / VIDEO INPUT
                       |
                       v
             FRAME CAPTURE MODULE
                       |
                       v
              IMAGE PREPROCESSING
                       |
                       v
             YOLOv8 / ONNX DETECTOR
                       |
                       v
              FIRE & SMOKE DETECTION
                       |
              +--------+--------+
              |                 |
              v                 v
         NO DETECTION       DETECTION
              |                 |
              v                 v
        Continue Video      Generate Alert
                                |
                                v
                         FASTAPI BACKEND
                                |
                 +--------------+--------------+
                 |              |              |
                 v              v              v
            Alert Manager  Camera Manager  User Manager
                 |              |              |
                 +--------------+--------------+
                                |
                                v
                           MYSQL DATABASE
                                |
                                v
                     PYQT5 DESKTOP APPLICATION
                                |
                 +--------------+--------------+
                 |              |              |
                 v              v              v
             Live View     Alert Dashboard   Map View
                                              |
                                              v
                                       LEAFLET MAP
```

## 3. Technology Stack

| Component | Technology |
|---|---|
| Programming Language | Python |
| Desktop Application | PyQt5 |
| Backend Framework | FastAPI |
| Deep Learning Model | YOLOv8 |
| Model Inference | ONNX Runtime |
| Computer Vision | OpenCV |
| Database | MySQL |
| ORM | SQLAlchemy |
| Database Migration | Alembic |
| Authentication | JWT |
| Password Hashing | bcrypt |
| Map Integration | Leaflet |
| API Server | Uvicorn |

## 4. Project Structure

```text
firevision_sql/
│
├── backend/
│   ├── core/
│   │   ├── config.py
│   │   ├── database.py
│   │   ├── security.py
│   │   └── logger.py
│   │
│   ├── models/
│   │   └── models.py
│   │
│   ├── schemas/
│   │   └── schemas.py
│   │
│   ├── routers/
│   │   ├── auth.py
│   │   ├── users.py
│   │   ├── cameras.py
│   │   └── alerts.py
│   │
│   ├── main.py
│   ├── requirements.txt
│   └── .env.example
│
├── software/
│   ├── app/
│   │   ├── ai/
│   │   │   ├── onnx_detector.py
│   │   │   └── export_yolo.py
│   │   │
│   │   ├── camera/
│   │   │   └── multiprocess_pipeline.py
│   │   │
│   │   ├── config/
│   │   │   └── settings.py
│   │   │
│   │   └── ui/
│   │       ├── login_dialog.py
│   │       ├── widgets.py
│   │       └── camera_location_manager.py
│   │
│   ├── main.py
│   └── requirements.txt
│
└── README.md
```

## 5. Installation and Setup

### Prerequisites

Make sure the following software is installed:

- Python 3.11 or later
- MySQL Server 8.0 or later
- Git

### Step 1: Clone the Repository

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd firevision_sql
```

### Step 2: Backend Setup

Create a virtual environment:

```bash
python -m venv .venv
```

Activate the environment on Windows:

```bash
.venv\Scripts\activate
```

Install backend dependencies:

```bash
cd backend
pip install -r requirements.txt
```

### Step 3: Configure Database

Create a MySQL database:

```sql
CREATE DATABASE firevision;
```

Configure the database connection and application settings in `backend/.env`.

Example:

```env
DATABASE_URL=mysql+mysqlconnector://root:password@localhost:3306/firevision

SECRET_KEY=your_secret_key
REFRESH_TOKEN_SECRET_KEY=your_refresh_secret

ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
REFRESH_TOKEN_EXPIRE_DAYS=7
```

### Step 4: Start the Backend

From the backend directory:

```bash
uvicorn main:app --reload --host 127.0.0.1 --port 8000
```

Backend API:

- API Documentation: http://localhost:8000/api/docs
- ReDoc: http://localhost:8000/api/redoc
- Health Check: http://localhost:8000/health

### Step 5: Desktop Application Setup

Open another terminal:

```bash
cd software
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```

Configure the backend URL in the desktop application's configuration:

```env
BACKEND_URL=http://localhost:8000
```

Start the desktop application:

```bash
python main.py
```

## 6. AI-Based Fire and Smoke Detection

FireVision Pro uses a YOLOv8-based deep learning model to identify fire and smoke in video frames.

### Detection Workflow

1. Capture frames from CCTV cameras or video sources.
2. Preprocess the captured frames.
3. Pass the frames to the YOLOv8 detection model.
4. Perform inference using ONNX Runtime.
5. Identify fire and smoke based on model predictions.
6. Generate detection results and alerts.
7. Display detection information in the desktop application.

### ONNX Model Export

If you have a trained YOLOv8 model (`best_m.pt`), export it into ONNX format:

```bash
python software/app/ai/export_yolo.py
```

The exported model is used by the ONNX detection engine.

### ONNX Execution Providers

The inference engine can use the following execution providers, depending on availability:

- CUDA Execution Provider – NVIDIA GPU acceleration.
- CPU Execution Provider – CPU-based inference.

## 7. Camera Monitoring and Alert Management

The camera monitoring module captures video frames and sends them to the AI detection engine.

The system supports:

- Live CCTV camera monitoring.
- Multiple camera configuration.
- Continuous frame processing.
- Fire and smoke detection.
- Detection alert generation.
- Alert status management.

A separate multiprocessing pipeline handles camera frame capture to reduce blocking of the desktop application's interface.

## 8. Geographical Map Integration

FireVision Pro integrates Leaflet to display camera locations and detected fire incidents on an interactive map.

The map module provides:

- Visualization of registered CCTV camera locations.
- Display of fire detection locations.
- Geographical identification of affected areas.
- Easy monitoring of incidents across different locations.

## 9. API Modules

The FastAPI backend provides REST APIs for communication between the desktop application and the database.

| Module | Functionality |
|---|---|
| Authentication | User login and token management |
| User Management | User registration and profile management |
| Camera Management | Camera registration and configuration |
| Alert Management | Fire alert creation and status updates |

### Main API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| POST | `/api/auth/token` | User login |
| POST | `/api/auth/refresh` | Refresh authentication token |
| POST | `/api/users/` | Register user |
| GET | `/api/users/me` | Get current user |
| GET | `/api/cameras/` | Retrieve cameras |
| POST | `/api/cameras/` | Register camera |
| GET | `/api/alerts/` | Retrieve alerts |
| POST | `/api/alerts/` | Create fire alert |
| PATCH | `/api/alerts/{id}` | Update alert status |
| GET | `/health` | Check backend status |

## 10. Expected Output

The FireVision Pro application provides the following outputs:

- Real-time CCTV video monitoring.
- Fire and smoke detection results.
- Visual identification of detected objects.
- Fire incident alerts.
- Camera location visualization.
- Centralized alert and camera management.

## 11. Future Enhancements

- Integration with additional CCTV camera networks.
- Improved detection accuracy under different lighting conditions.
- SMS, email, and mobile notifications.
- Fire severity classification.
- Historical fire incident analytics.
- Automated emergency response integration.

## 12. Conclusion

FireVision Pro combines computer vision, deep learning, desktop application development, and backend technologies to create an integrated fire and smoke detection platform. By combining real-time CCTV monitoring, AI-based detection, centralized alert management, and geographical visualization, the system aims to support faster identification and monitoring of potential fire incidents.

---

**FireVision Pro – AI-Powered Real-Time Fire and Smoke Detection System.**
