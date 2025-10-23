```
from ultralytics import YOLO
from picamera2 import Picamera2
import cv2
import time

# Load YOLOv8 Nano (smallest and fastest)
model = YOLO("yolov8n.pt")

# Initialize Pi camera
picam2 = Picamera2()
picam2.preview_configuration.main.size = (640, 480)
picam2.preview_configuration.main.format = "RGB888"
picam2.configure("preview")
picam2.start()

print("Starting YOLO real-time detection... Press 'q' to quit.")

while True:
    frame = picam2.capture_array()
    
    # Run YOLO inference
    results = model.predict(frame, conf=0.5, verbose=False)
    
    # Draw results on frame
    annotated = results[0].plot()

    # Display
    cv2.imshow("YOLOv8 Detection", annotated)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cv2.destroyAllWindows()
```
```
python3 -c "import torch; print(torch.__version__)"
python3 -c "import torchvision; print(torchvision.__version__)"
python3 -c "import ultralytics; print('Ultralytics installed!')"
```
