
import cv2 as cv
import numpy as np
 
cap = cv.VideoCapture(0)
prevCircle = None
dist = lambda x1,y1,x2,y2: (x1-x2)*2(y1-y2)**2
 
while True:
    _, frame = cap.read()
    hsv_frame = cv.cvtColor(frame, cv.COLOR_BGR2HSV)
    # Kırmızı Renk
    low_red = np.array([161, 155, 84])
    high_red = np.array([179, 255, 255])
    red_mask = cv.inRange(hsv_frame, low_red, high_red)
    red = cv.bitwise_and(frame, frame, mask=red_mask)
    
       # Mavi Renk
       low_blue = np.array([94, 80, 2])
       high_blue = np.array([126, 255, 255])
       blue_mask = cv.inRange(hsv_frame, low_blue, high_blue)
       blue = cv.bitwise_and(frame, frame, mask=blue_mask)
 
     # Yeşil Renk
     low_green = np.array([25, 52, 72])
     high_green = np.array([102, 255, 255])
     green_mask = cv.inRange(hsv_frame, low_green, high_green)
     green = cv.bitwise_and(frame, frame, mask=green_mask)
 
     # Every color except white
     low = np.array([0, 42, 0])
     high = np.array([179, 255, 255])
     mask = cv.inRange(hsv_frame, low, high)
     result = cv.bitwise_and(frame, frame, mask=mask)
    
    cv.imshow("Frame", frame)
    cv.imshow("Red", red)
    cv.imshow("Blue", blue)
    cv.imshow("Green", green)
    cv.imshow("Result", result)
 
    key = cv.waitKey(1)
    if key == 27:
        break
    ret, frame = cap.read()
    if not ret: break
 

    grayFrame = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    blurFrame = cv.GaussianBlur(grayFrame, (17,17),0)
    
    circles = cv.HoughCircles(blurFrame, cv.HOUGH_GRADIENT, 1.2,150,
                              param1=100, param2=30, minRadius =75,maxRadius=400)
    
    if circles is not None:
        circles = np.uint16(np.around(circles))
        chosen = None
        for i in circles[0, :]:
            if chosen is None: chosen=i
            if prevCircle is not None:
                if dist(chosen[0], chosen[1],prevCircle[0],prevCircle[1])<= dist(i[0],i[1],prevCircle[0],prevCircle[1]):
                    chosen = i
                    
        cv.circle(frame,(chosen[0],chosen[1]),1,(0,100,100),3)
        cv.circle(frame,(chosen[0],chosen[1]),chosen[2],(255,0,255),3)
        prevCircle= chosen
        
    cv.imshow("circles",frame)
    
    if cv.waitKey(1) & 0xFF == ord('q'): break
