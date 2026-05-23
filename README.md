# Driver Drowsiness Detection System

An AI-powered real-time drowsiness detection system designed to monitor eye movements and detect signs of fatigue using computer vision and facial landmark analysis. The system uses OpenCV, Dlib, and pyttsx3 to identify prolonged eye closure and provide instant voice alerts for improved safety and attention monitoring.

## Features

* Real-time face and eye detection
* Eye Aspect Ratio (EAR)–based drowsiness detection
* Instant audio alert using speech synthesis
* Live webcam monitoring and visual warning system
* Facial landmark tracking using Dlib
* Efficient and responsive fatigue detection

## Tech Stack

* Python
* OpenCV
* Dlib
* NumPy
* pyttsx3

## Working Principle

The system captures live video through a webcam and detects facial landmarks using Dlib’s 68-point face landmark predictor. Eye landmarks are extracted to calculate the Eye Aspect Ratio (EAR), which measures eye openness. When the EAR falls below a predefined threshold for a certain duration, the system identifies drowsiness and triggers visual and voice alerts in real time.

## Use Cases

* Driver safety and accident prevention
* Fatigue monitoring systems
* Smart surveillance applications
* Workplace safety monitoring
* AI-based attention tracking systems

## Project Goal

The primary objective of this project is to develop a reliable, cost-effective, and non-intrusive drowsiness detection system that enhances human safety through real-time monitoring, computer vision, and intelligent alert mechanisms.
