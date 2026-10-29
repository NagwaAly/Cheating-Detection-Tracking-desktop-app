# Cheating-Detection-Tracking-desktop-app
An AI-powered desktop application for monitoring students during exams using computer vision, object tracking, behavior analysis, and database integration.

The system processes exam videos, tracks students across frames, detects selected suspicious behaviors, generates alerts, captures screenshots, and stores detected events in a SQL Server database.

## Project Overview

The project was developed as an end-to-end AI pipeline rather than only a standalone detection model.

The workflow includes:

```text
Exam Video
    ↓
Student Detection
    ↓
Student Tracking
    ↓
Behavior Signal Analysis
    ↓
Cheating Decision Logic
    ↓
Alert Generation
    ↓
Screenshot Capture
    ↓
SQL Server Storage
    ↓
Tkinter Desktop Interface
```

## Main Features

- Student detection using YOLO
- Multi-object student tracking using ByteTrack
- Persistent Track IDs for students
- Mobile phone detection
- Head-direction analysis
- Detection of prolonged sideways looking
- Detection of repeated head turns
- Rule-based suspicious behavior decision
- Green bounding boxes for normal behavior
- Red bounding boxes for suspected cheating behavior
- Automatic screenshot capture when an alert is generated
- SQL Server integration for storing detected events
- Desktop interface developed using Tkinter
- Video upload and processing through the GUI
- Alert history display
- Screenshot viewer for detected events

## Technologies Used

- Python
- YOLO11
- Ultralytics
- OpenCV
- ByteTrack
- YOLO Pose
- Tkinter
- SQL Server
- PyODBC
- Pandas
- NumPy
- Computer Vision

## Project Development Stages

The system was developed incrementally through several stages.

### 1. Student Detection

YOLO was used to detect students in exam videos.

Each detected student is represented using a bounding box.

### 2. Student Tracking

ByteTrack was used to assign Track IDs to students and maintain their identities across video frames.

The tracker configuration was tuned to reduce unnecessary ID fragmentation.

### 3. Behavior Signal Extraction

The system extracts several behavioral indicators for each tracked student.

These include:

- Head direction
- Side-looking duration
- Number of repeated head turns
- Mobile phone detection

These signals are tracked over time rather than making decisions from individual frames only.

## Suspicious Behavior Logic

The current system detects three primary suspicious behaviors.

### Prolonged Sideways Looking

A student may be flagged when the detected head direction remains sideways for longer than the configured time threshold.

### Repeated Head Turns

Repeated changes in head direction within a defined time window may generate an alert.

### Mobile Phone Detection

A phone must be detected repeatedly across a short temporal window before generating an alert.

This reduces alerts caused by isolated detections.

## Alert Decision

The decision layer combines the extracted behavioral signals.

Example logic:

```text
Phone detected continuously
        OR
Prolonged sideways looking
        OR
Repeated head turns
        ↓
Suspected Cheating
```

Normal students are displayed using a green bounding box.

Students associated with suspicious behavior are displayed using a red bounding box.

## Alert Information

When suspicious behavior is detected, the system stores information such as:

- Student Track ID
- Behavior detected
- Confidence score
- Video timestamp
- Event date and time
- Screenshot path
- Head direction
- Side-looking duration
- Head-turn count
- Phone detection information

## Screenshot Capture

When a suspicious event is generated, the application automatically captures the current processed frame.

The screenshot can later be reviewed from the desktop application.

## Database

SQL Server is used to store video information and cheating alerts.

Database used during development:

```text
ExamProctoringDB
```

Main tables:

```text
ExamVideos
CheatingEvents
```

### ExamVideos

Stores information about processed exam videos such as:

- Video name
- Video path
- Duration
- FPS
- Number of frames
- Processing status

### CheatingEvents

Stores detected suspicious events including:

- Student Track ID
- Behavior
- Confidence score
- Video timestamp
- Event timestamp
- Screenshot path
- Head direction
- Side-looking duration
- Head-turn count
- Phone detection information

## Desktop Application

A Tkinter desktop interface was developed to make the system easier to use.

The main application workflow is:

```text
Upload Video
    ↓
Start Processing
    ↓
AI Analysis
    ↓
Visual Results
    ↓
Alert Generation
    ↓
Database Storage
```

The interface provides:

- Upload Video
- Start Processing
- Stop Processing
- Live processed video display
- Alert History
- Screenshot Viewer
- SQL Server integration

## Bounding Box Colors

```text
Green  → Normal behavior

Red    → Suspected cheating behavior

Orange → Detected mobile phone
```

## Project Structure

The project was developed using separate notebooks for the main stages.

```text
01_student_detection_yolo.ipynb

02_student_tracking_bytetrack.ipynb

02B_tracking_evaluation_and_stabilization.ipynb

03_cheating_behavior_signals.ipynb

04_final_cheating_decision_alerts_screenshots.ipynb

04B_robust_alert_state_and_phone_window.ipynb

05_sql_server_integration.ipynb

06_tkinter_desktop_app.ipynb
```

## Example Workflow

1. Select an exam video.
2. Upload the video through the desktop application.
3. Click **Start Processing**.
4. Students are detected and tracked.
5. Behavioral signals are analyzed.
6. Suspicious behavior is highlighted in red.
7. A screenshot is automatically captured.
8. The event is stored in SQL Server.
9. The event can be reviewed from the Alert History.

## Current Detection Scope

The current version focuses on:

- Mobile phone usage
- Prolonged sideways head orientation
- Repeated head turning

The system currently relies mainly on body/head-related visual signals and object detection.

Eye-gaze-only behavior is not part of the stable final version.

## Limitations

This project is a prototype and should not be treated as a fully autonomous cheating-detection system.

Potential limitations include:

- Tracking ID fragmentation
- Occlusion between students
- Low-resolution video
- Camera angle
- Lighting conditions
- False positive phone detections
- Head-pose estimation errors
- Suspicious behavior does not necessarily mean confirmed cheating

Detected events should therefore be considered alerts for human review rather than final disciplinary decisions.

## Future Improvements

Possible future improvements include:

- Dedicated eye-gaze estimation
- Face orientation estimation
- Improved re-identification
- Multi-camera support
- More advanced temporal behavior modeling
- Deep-learning-based behavior classification
- Real-time webcam monitoring
- Notification system
- User authentication
- Improved dashboard and reporting
- Exportable examination reports

## Purpose

The main objective of this project is to demonstrate how computer vision can be integrated into a complete software system involving:

- AI inference
- Object tracking
- Temporal behavior analysis
- Decision logic
- Database storage
- Desktop application development

## Disclaimer

This project is intended for educational and research purposes.

Detected suspicious behavior should be reviewed by a human supervisor before making any final decision.
