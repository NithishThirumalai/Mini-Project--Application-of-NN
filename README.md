## MINI PROJECT - APPLICATION OF NN

### PROJECT TITLE
Vehicle detection

### PROJECT DESCRIPTION
A Vehicle Detection application made using OpenCV. In this project I have built an OpenCV application in which a user can detect vehicles in a video or through web camera.

### ALGORITHM
- START
- Input the video / image
- Fix the frame size
- Fix the line position
- Detect the object and count
- Display the count
- Iterates the process
- STOP

### PROGRAM
```import cv2
import numpy as np

#Web Camera
cap = cv2.VideoCapture('assets/video.mp4')

min_width_rectangle = 80
min_height_rectangle = 80

count_line_position = 550

# Initialize Substructor
algo = cv2.createBackgroundSubtractorMOG2(history=100, varThreshold=40)

def center_handle(x,y,w,h):
    x1=int(w/2)
    y1=int(h/2)
    cx=x+x1
    cy=y+y1
    return cx,cy

detect = []
offset = 6 #Alowable error b/w pixel
counter = 0

while True:
    ret, video = cap.read()
    gray = cv2.cvtColor(video, cv2.COLOR_BGR2GRAY)
    blur = cv2.GaussianBlur(gray, (3,3), 5)
    
# Applying on each frame
    vid_sub = algo.apply(blur)
    dilat = cv2.dilate(vid_sub, np.ones((5,5)))
    kernel = cv2.getStructuringElement(cv2.MORPH_ELLIPSE,(5,5))
    dilatada = cv2.morphologyEx(dilat, cv2.MORPH_CLOSE, kernel)
    dilatada = cv2.morphologyEx(dilatada, cv2.MORPH_CLOSE, kernel)
    countersahpe, h = cv2.findContours(dilatada, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)

    cv2.line(video, (25,count_line_position),(1200,count_line_position),(255,0,0), 3)

    for (i, c) in enumerate(countersahpe):
        (x,y,w,h) = cv2.boundingRect(c)
        val_counter = (w>=min_width_rectangle) and (h>= min_height_rectangle)
        if not val_counter:
            continue
        cv2.rectangle(video,(x,y),(x+w,y+h),(0,255,255),2)
        cv2.putText(video,"Vehicle No: " + str(counter), (x,y-20),cv2.FONT_HERSHEY_TRIPLEX,1,(255,244,0),2)


        center = center_handle(x,y,w,h)
        detect.append(center)
        cv2.circle(video, center, 4, (0,0,255), -1)

        for (x,y) in detect:
            if y<(count_line_position + offset) and  y>(count_line_position - offset):
                counter+=1
                cv2.line(video, (25,count_line_position),(1200,count_line_position),(0,127,255), 3)
                detect.remove((x,y))

                print("Vehicle No: "+ str(counter))

    cv2.putText(video,"Vehicle No: " + str(counter), (450,70),cv2.FONT_HERSHEY_SIMPLEX,2,(0,0,255),5)

    cv2.imshow('Detector',video)


    if cv2.waitKey(1) == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```
### OUTPUT 

<img src="https://user-images.githubusercontent.com/114301782/206269831-21eaa3b4-b455-4a59-9eec-6eeecd8e2841.jpeg" width="600">

<img src="https://user-images.githubusercontent.com/114301782/206269955-e928beeb-58b4-4bc6-88a2-26a0784547ff.jpeg" width="600">

<img src="https://user-images.githubusercontent.com/114301782/206270012-e90f7a54-96c1-4f51-94f7-4c63744a644a.jpeg" width="600">

### ADVANTAGE 
Vehicle detection is completely inter-linked with other similar computer vision techniques such as image segmentation and image recognition that assist us to understand and analyze the scenes in videos and images.

### RESULT
Thus, The Vehicle detection is implemented successfully.
