# 🎯 Face Recognition Attendance System

![Python](https://img.shields.io/badge/Python-3.8+-blue?style=flat-square&logo=python)
![OpenCV](https://img.shields.io/badge/OpenCV-4.5.4-green?style=flat-square&logo=opencv)
![Firebase](https://img.shields.io/badge/Firebase-RealtimeDB-orange?style=flat-square&logo=firebase)

An automated attendance tracking system powered by facial recognition technology and Firebase real-time database.

## ✨ Features

- 📸 **Real-time face detection and recognition** - Uses `face_recognition` library for accurate face detection
- ✅ **Automated attendance marking** - Automatically records attendance when a face is recognized
- 🔥 **Firebase real-time database integration** - Stores all data in Firebase RTDB
- 👤 **Student information display** - Shows student details on the UI
- 🔄 **Duplicate entry prevention** - 30-second cooldown between attendance marks
- ⚡ **Multi-threaded performance** - Background threading for database updates

## 🚀 Getting Started

### Prerequisites

| Requirement | Description |
|-------------|-------------|
| Python | Version 3.8 or higher |
| Webcam | Any working webcam |
| Firebase Account | For database storage |

### Required Libraries

```bash
pip install opencv-python
pip install face-recognition
pip install numpy
pip install firebase-admin
pip install cvzone
```

Or install all dependencies at once:

```bash
pip install -r requirements.txt
```

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/Rittik2002/Real_Time_Attendance.git
   cd Real_Time_Attendance
   ```

2. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Firebase Configuration**
   - Go to [Firebase Console](https://console.firebase.google.com/)
   - Create a new project
   - Create a Realtime Database
   - Go to Project Settings > Service Accounts
   - Generate new private key (download `serviceaccountkey.json`)
   - Place `serviceaccountkey.json` in the project root
   - Update the `databaseURL` in `main.py` with your Firebase URL

4. **Add Student Data to Database**
   ```bash
   python addDataToDataBase.py
   ```

5. **Generate Face Encodings**
   ```bash
   python encodegenerator.py
   ```
   > ⚠️ Ensure student images are added to `Images/Students/` folder before running this

6. **Run the Application**
   ```bash   python main.py
   ```

## 📁 Project Structure

```
Real_Time_Attendance/
├── main.py                     # Main application entry point
├── encodegenerator.py          # Generates face encodings from images
├── addDataToDataBase.py        # Initializes Firebase with student data
├── Encodefile.p                # Pickle file storing face encodings
├── requirements.txt            # Python dependencies
├── readme.md                   # This file
├── serviceaccountkey.json      # Firebase credentials (you need to add this)
├── Images/
│   ├── Background/
│   │   └── full.jpg            # Main UI background
│   ├── Students/               # Student images (add your images here)
│   │   └── student_*.jpg
│   └── state/
│       ├── 1.jpg               # Default state
│       ├── 2.jpg               # Processing state
│       ├── 3.jpg               # Already marked state
│       └── 4.jpg               # Unknown face state
```

## 🗄️ Database Structure

```json
{
    "Students": {
        "12345": {
            "name": "John Doe",
            "major": "Computer Science",
            "starting_year": "2023",
            "total_attendence": 0,
            "standing": "Good",
            "Year": 1,
            "last_attendence_time": "2024-01-15 09:30:00"
        }
    }
}
```

## ⚙️ How It Works

```
┌─────────────┐    ┌─────────────────┐    ┌──────────────────┐
│  Webcam     │───>│  Face Detection │───>│  Face Encoding   │
│  Capture    │    │  (face_recognition)    │  Generation      │
└─────────────┘    └─────────────────┘    └────────┬─────────┘
                                                   │
                     ┌─────────────────────────────┼─────────────────────┐
                     │                             │                     │
                     ▼                             ▼                     ▼
          ┌──────────────────┐         ┌──────────────────┐    ┌──────────────────┐
          │  Match Found    │         │   Cooldown      │    │  Unknown Face    │
          │  Update Firebase│         │   Check (30s)   │    │   Display UI     │
          └──────────────────┘         └──────────────────┘    └──────────────────┘
```

1. **Capture** - Webcam captures video frames continuously
2. **Detect** - Face detection identifies faces in each frame
3. **Encode** - Generate unique encoding for each detected face
4. **Match** - Compare with stored encodings in `Encodefile.p`
5. **Update** - If match found and cooldown passed, update Firebase
6. **Display** - Show student information on the UI

## 🔧 Configuration

### Changing the Cooldown Period

Edit `main.py` to adjust the cooldown time (in seconds):

```python
if time_second > 30:  # Change 30 to your desired cooldown
```

### Changing UI Layout

Edit the coordinates in `main.py`:

```python
# Webcam display area
x, y, w, h = 50, 140, 600, 460

# Mode display area
x_mode, y_mode, w_mode, h_mode = 800, 90, 300, 500
```

## 🛠️ Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| `ModuleNotFoundError` | Install all dependencies: `pip install -r requirements.txt` |
| `Firebase connection error` | Check your `serviceaccountkey.json` and database URL |
| `No faces detected` | Ensure proper lighting and face is visible |
| `Encoding file not found` | Run `python encodegenerator.py` first |
| `Webcam not working` | Check if webcam is accessible/ permissions granted |

### Tips for Better Recognition

- ✅ Use good lighting (avoid shadows on face)
- ✅ Position face directly in front of camera
- ✅ Use clear, front-facing student photos
- ✅ Avoid glasses or change them for consistent photos

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch
   ```bash
   git checkout -b feature/AmazingFeature
   ```
3. Commit your changes
   ```bash
   git commit -m 'Add some AmazingFeature'
   ```
4. Push to the branch
   ```bash
   git push origin feature/AmazingFeature
   ```
5. Open a Pull Request

## 📝 License

This project is for educational purposes. Modify and use it as needed.



