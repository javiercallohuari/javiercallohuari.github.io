import cv2
import numpy as np

droidcam_url = "http://192.168.0.10:4747/mjpegfeed"
cap = cv2.VideoCapture(droidcam_url)

traza = None

lower_red = np.array([160, 100, 50])
upper_red = np.array([180, 255, 255])

while True:
  
    ret, frame = cap.read()

    if not ret:
        print("No se pudo capturar el frame. ¿Está DroidCam ejecutándose?")
        break

    if traza is None:
        traza = np.zeros_like(frame)

    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

    mask = cv2.inRange(hsv, lower_red, upper_red)

    contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

    for contour in contours:
        if cv2.contourArea(contour) > 500:
            cv2.drawContours(traza, [contour], -1, (0, 0, 255), 2)

    frame = cv2.add(frame, traza)

    cv2.imshow('Deteccion de Luz Roja y Dibujo de Figuras Geometricas', frame)

    key = cv2.waitKey(1) & 0xFF
    if key == ord('q'):
        break
    elif key == ord('c'):
       
        traza = np.zeros_like(frame)

cap.release()
cv2.destroyAllWindows()
