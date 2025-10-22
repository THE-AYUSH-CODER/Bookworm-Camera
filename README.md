# Bookworm-Camera
If you’re using Raspberry Pi OS Bookworm, so we’ll install OpenCV + Picamera2 (the new official camera API).
This method works perfectly with both Pi Camera Module and USB cameras, and gives you an OpenCV live preview window.


---

⚙️ STEP 1 — Update your system

Open a terminal and run:
```
sudo apt update
sudo apt upgrade -y
sudo reboot
```
(After reboot, open the terminal again.)


---

📦 STEP 2 — Install required dependencies

Run:
```
sudo apt install -y python3-opencv python3-picamera2 libcamera-apps
```
✅ This installs:

- opencv-python (cv2 module)

- picamera2 (official Pi camera library)

- libcamera backend (for the camera hardware)


To confirm installation:
```
python3 -c "import cv2; print(cv2.__version__)"
```
You should see a version like 4.x.x.


---

📸 STEP 3 — Check camera connection

Make sure your camera ribbon is properly seated in the CSI port near the HDMI connectors.
Then test the camera:
```
rpicam-hello --list-cameras
```
If you see something like:
```
Available cameras:
0 : imx708 [4608x2592] (/base/soc/i2c0mux/i2c@1/imx708@1a)
```
✅ your camera is detected.

If not — recheck the cable or run:
```
dmesg | grep imx
```

---

🧠 STEP 4 — Create a Python test file

Create a new file:
```
nano camera_preview.py
```
Paste this code:
```
from picamera2 import Picamera2
import cv2

# Initialize camera
picam2 = Picamera2()

# Set preview resolution and format
picam2.preview_configuration.main.size = (640, 480)
picam2.preview_configuration.main.format = "RGB888"
picam2.configure("preview")

# Start camera
picam2.start()

while True:
    # Capture frame as numpy array
    frame = picam2.capture_array()

    # Display using OpenCV
    cv2.imshow("Camera Preview", frame)

    # Press 'q' to quit
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cv2.destroyAllWindows()
```
Save and exit (Ctrl + O, then Enter, then Ctrl + X).


---

▶️ STEP 5 — Run your program

In terminal:
```
python3 camera_preview.py
```
✅ You’ll see a live preview window.
Press Q to quit.


---

🎥 STEP 6 — (Optional) Capture photos or record video

To save frames:
```
cv2.imwrite("photo.jpg", frame)
```
To record video, add a cv2.VideoWriter object.
I can give you a ready script for recording if you want.


---

🧩 Troubleshooting

| **Issue** | **Fix** |
|-----------|---------|
| ModuleNotFoundError: No module named 'picamera2' | Run ```sudo apt install python3-picamera2 -y``` |
| libcamera: No cameras available | Check ribbon cable and rerun ```rpicam-hello --list-cameras``` |
| Window doesn’t open | Make sure you’re running from the desktop (not over SSH without X forwarding) |


---

✅ Summary

| **Step**                      | **Command**                                                             |
|------------------------------|--------------------------------------------------------------------------|
| Update system                | ```sudo apt update && sudo apt upgrade -y```                                 |
| Install OpenCV + camera libs | ```sudo apt install python3-opencv python3-picamera2 libcamera-apps -y```   |
| Test camera                  | ```rpicam-hello --list-cameras```                                            |
| Run OpenCV preview           | ```python3 camera_preview.py```                                              |



---

