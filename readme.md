## Intelligent Face Recognition and Liveness-Based Attendance System
This project is a robust, multi-layer attendance solution built using Python and computer vision libraries. It provides accurate identity verification, ensures physical presence through liveness detection (blink/movement analysis), enforces health compliance (mask detection), and manages real-time attendance logging across multiple database backends (MySQL and Google Sheets).

# Key Features
1. Multi-Layer Biometric Verification
Face Recognition: Utilizes the DeepFace library with the ArcFace model for highly accurate, state-of-the-art facial embedding generation and matching.

FAISS Integration: Incorporates FAISS (Facebook AI Similarity Search) for lightning-fast nearest-neighbor search, ensuring quick identification even with large facial databases.

Liveness Detection (Anti-Spoofing):

Blink Detection: Uses MediaPipe Face Mesh to calculate the Eye Aspect Ratio (EAR), requiring a minimum number of blinks to confirm a live user.

Micro-Movement Detection: Analyzes subtle shifts in key facial landmarks to detect static images or video playback fraud, automatically pausing the system if a face remains unnaturally still for too long.

2. Health & Compliance Check
Mask Detection: Integrates a pre-trained TensorFlow/Keras model to detect if a person is wearing a mask, preventing attendance logging until the mask is removed.

3. Robust Data Management
Dual Backend Logging: Logs attendance records concurrently to two separate systems:

MySQL Database: Provides local, structured storage and is used for attendance interval checks and calculating daily lecture counts.

Google Sheets: Offers cloud-based, collaborative data storage for easy sharing and accessibility.

Section Management: The system supports compartmentalized attendance for different groups (Sections A, B, C), each with its own face database (.pkl file) and MySQL table.

4. User Experience & Control
New User Registration: Includes a dedicated, guided flow to register new users by capturing multiple face images to build a reliable embedding profile.

Attendance Interval Restriction: Prevents duplicate attendance logging by enforcing a configurable interval (default: 1 hour) between successful marks for the same user.

Interactive Menu: Features a command-line/video overlay menu for switching between attendance sections (a, b, c) and initiating new user registration (n).

## Technology Stack
Language: Python

Computer Vision: opencv-python, mediapipe

Deep Learning / Face Analysis: deepface (ArcFace model), tensorflow/keras (for mask detection)

Vector Search: faiss-cpu

Database: mysql.connector

Cloud Integration: gspread, oauth2client (for Google Sheets API)

Utilities: numpy, scipy, pickle

## Installation and Setup
Prerequisites
Python 3.x

MySQL Server: Must be running locally or accessible via network.

Google Sheets API Credentials:

Enable the Google Drive and Google Sheets APIs.

Download the service account key file and name it credentials.json in the project root.

Share your target Google Sheet (named Face_Attendance) with the service account email found in credentials.json.

## Setup Steps
Clone the repository:

git clone [(https://github.com/harwinder766/face-recognition-attendance.git)]
cd [face-recognition-attendance-system]

Install dependencies:

pip install -r requirements.txt
Note: If you have a specific GPU setup, use the appropriate deepface and tensorflow versions.

Configure Constants:
Adjust database credentials and thresholds in the main script/notebook. Ensure you update the placeholder password:

Database Settings
DB_HOST = "localhost"
DB_USER = "root"
DB_PASSWORD = "YOUR_SECURE_PASSWORD" # <<< CHANGE THIS!
DB_NAME = "attendance_db"

Thresholds
SIMILARITY_THRESHOLD = 0.63
LIVENESS_BLINKS_REQUIRED = 2 
ATTENDANCE_INTERVAL_HOURS = 1

Run the application:

python notebook.ipynb # or main_script.py if you convert the notebook

## How to Use
The application starts in a main menu mode displayed on the webcam feed.

Register a New User: Press n and follow the prompts to enter a name and section. The system will guide you through capturing 20 unique face images to build your profile.

Start Attendance: Press a, b, or c to activate the attendance mode for the corresponding section.

Mark Attendance: Once in attendance mode, face the camera. The system will:

Identify you (label and similarity score).

Check for a mask (requires "No Mask").

Prompt for liveness (requires 2 blinks and sufficient head movement).

If all checks pass, your attendance will be logged to MySQL and Google Sheets.

Pause/Quit: Press p to pause detection and return to the main menu, or q to quit the entire application.

