# Videos


## From Webcam

```python
import cv2

# Connects to your computer's default camera
cap = cv2.VideoCapture(0)

while True:
    # Capture frame-by-frame
    ret, frame = cap.read()

    # Our operations on the frame come here
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    cv2.imshow('frame', gray)
    
    # quit with "q"
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# When everything done, release the capture and destroy the windows
cap.release()
cv2.destroyAllWindows()
```

### Get Latest Frames

OpenCV's videocapture's read() only reads the frames sequentially. 
However, we might want to acquire the latest frame after processing the previous one,
else the latency will quickly build up overtime.

A helpful code snippet from stackoverflow can help to resolve this:
https://stackoverflow.com/questions/51722319/skip-frames-and-seek-to-end-of-rtsp-stream-in-opencv

```python
import threading
from threading import lock

class Camera:
    """Read the latest frame from RTSP video stream.
    A daemon thread is produced that runs the stream in the background.
    When required, getFrame() will grab the latest copy of the frame from the thread.
    """

    last_frame = None
    last_ready = None
    lock = Lock()

    def __init__(self, rtsp_link):
        capture = cv2.VideoCapture(rtsp_link)
        thread = threading.Thread(
            target=self.rtsp_cam_buffer, args=(capture,), name="rtsp_read_thread"
        )
        thread.daemon = True
        thread.start()

    def rtsp_cam_buffer(self, capture):
        while True:
            with self.lock:
                self.last_ready, self.last_frame = capture.read()

    def getFrame(self):
        if (self.last_ready is not None) and (self.last_frame is not None):
            return self.last_frame.copy()
        else:
            return None
```

This is how we call the class in.

```python
uri = 'mask.mp4'
capture = Camera(uri)
while True:
    frame = capture.getFrame()
    if frame is not None:
        cv2.imshow('frame',frame)
    else:
        print('frame is None')

    if cv2.waitKey(25) & 0xFF == ord('q'):            
        break
```


## From File

```python
import cv2
import time

cap = cv2.VideoCapture('../DATA/video_capture.mp4')

# FRAMES PER SECOND
fps = 25

# check for video file
if cap.isOpened()== False: 
    print("Error opening the video file")
    

# While the video is opened
while cap.isOpened():
    
    # Read the video file
    ret, frame = cap.read()
    
    # If we got frames, show them.
    if ret == True:

        # Display the frame at same frame rate of recording
        time.sleep(1/fps)
        cv2.imshow('frame',frame)

        if cv2.waitKey(25) & 0xFF == ord('q'):            
            break

    # automatically break this whole loop if the video is over
    else:
        break
        
cap.release()
# Closes all the frames
cv2.destroyAllWindows()
```