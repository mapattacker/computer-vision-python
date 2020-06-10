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