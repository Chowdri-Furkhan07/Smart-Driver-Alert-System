# Drowsiness_detection
This project focuses on developing a real-time drowsiness detection system using OpenCV, Dlib, and pyttsx3. The system aims to identify signs of fatigue or drowsiness in individuals, particularly drivers, by monitoring their eye movements and providing an instant audio alert when drowsiness is detected.

The project uses computer vision and facial landmark detection techniques to recognize eye closure patterns. A webcam continuously captures video frames, which are converted to grayscale for efficient processing. Dlib’s frontal face detector identifies faces in the frame, and the shape predictor model (shape_predictor_68_face_landmarks.dat) maps 68 specific facial landmarks, including eye coordinates.

To detect drowsiness, the system calculates the Eye Aspect Ratio (EAR) — a metric derived from the Euclidean distances between vertical and horizontal eye landmarks. If the eyes are open, the EAR value remains relatively high; however, when the eyes close or droop due to drowsiness, the EAR drops significantly.

When the average EAR of both eyes falls below a defined threshold (e.g., 0.25), the system recognizes this as a sign of drowsiness. It then overlays visual warnings on the screen—such as “DROWSINESS DETECTED” and “Alert! WAKE UP DUDE”—and simultaneously uses pyttsx3 to deliver a voice alert, ensuring the person becomes aware and reacts immediately.

The program continuously monitors the user until manually stopped. It can be fine-tuned by adjusting the EAR threshold or duration of eye closure detection for different environments or lighting conditions.

This project has practical applications in driver safety systems, health monitoring, and industrial automation, where constant attention is critical. It represents an integration of AI-based facial recognition, image processing, and speech synthesis to enhance human safety and prevent fatigue-related accidents.

By combining automation and real-time alerting, this system provides a cost-effective, non-intrusive, and reliable solution to address drowsiness detection challenges in daily life.
