# IE482-HW2-SCIME

## Yolo V8 Vehicle/Item Tracker
This CV alogrithm is very cool as it allows the video feed to identify numerous objects including: Cell phones, people, cars, animals and other misc objects. Mainly the software can indenify cars, trace their movement path and can actually idetify color, make and model.

## Setup
- Most infromation can be found on the following github: [Vehicle Detection Powered by YOLO](https://github.com/sergio11/vehicle_detection_tracker.git)
- The first step is to open windows power shell and run
  ```
  pip install VehicleDetectionTracker
  ```
- This will install the YOLOv8 and Vehicle tracker into you python program files
- Next you will need to make sure you have PyTorch version 2.5.0
  - The newer version will give you an UnpicklingError i found it easiest to downgrade PyTorch
- First if you have the new version run:
```
pip uninstall torch torchvision torchaudio
```
-Then run:
```
pip install torch==2.5.0 torchvision torchaudio
```
- Next what i did was implent the following code into out current streaming python note book
```
from VehicleDetectionTracker.VehicleDetectionTracker import VehicleDetectionTracker

import cv2

video_path = "https://localhost:8000/stream.mjpg"

# Open the video stream
cap = cv2.VideoCapture(video_path)
    
if not cap.isOpened():
    print("Error: Could not open video stream")
else:
    while True:
        ret, frame = cap.read()
        if not ret:
            print("Failed to grab frame")
            break
        
        cv2.imshow("Stream", frame)  # Display the video stream

        if cv2.waitKey(1) & 0xFF == ord('q'):  # Press 'q' to quit
            break

cap.release()
cv2.destroyAllWindows()

vehicle_detection = VehicleDetectionTracker()
result_callback = lambda result: print({
    "number_of_vehicles_detected": result["number_of_vehicles_detected"],
    "detected_vehicles": [
        {
            "vehicle_id": vehicle["vehicle_id"],
            "vehicle_type": vehicle["vehicle_type"],
            "detection_confidence": vehicle["detection_confidence"],
            "color_info": vehicle["color_info"],
            "model_info": vehicle["model_info"],
            "speed_info": vehicle["speed_info"]
        }
        for vehicle in result['detected_vehicles']
    ]
})
    
vehicle_detection.process_video(video_path, result_callback)
```
- This block off code can go right after the `camera.Startscript` line
- Now for my system (Windows 11 > MAC) I needed to alter the `VehicleDetectionTracker.py` code
- Locate `VehicleDetectionTracker.py` in file manager use
```
pip show VehicleDetectionTracker
```
- Next open that file and go to line 198 and change `delta_t = int(t2) - int(t1).total_seconds()` to
```
delta_t = (t2 - t1).total_seconds()
```

# Running The Code
- Navigate to where you have the ub_camera files located and open the command window
- Next open juypter note book using
```
python -m notebook
```
- Open the code where you run the webcam
- run everyting up to the new chunk of code for the vehicle detection CV algoritm
- Run the new chunk of code
- This opens the webcam in a python window
- Press Q on your keyboard to close this window
- Once this window closes out the YOLOv8 window should pop up
- When the system sees a car it will show you the color make and model in the jupyter notebook page
## QUE EPIC DEMO VIDEO
[![Watch the video](https://i.sstatic.net/Vp2cE.png)](https://ubuffalo-my.sharepoint.com/:v:/r/personal/liamscim_buffalo_edu/Documents/VIDEO%20DEMO.mp4?csf=1&web=1&e=XcsE93)
