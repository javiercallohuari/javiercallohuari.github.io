import cv2

image = cv2.imread("descarga.jpg")

upsampled_image = cv2.resize(image, dsize=(image.shape[1] * 2, image.shape[0] * 2), interpolation=cv2.INTER_CUBIC)

cv2.imshow("Imagen original", image)

cv2.imshow("Imagen ampliada", upsampled_image)

cv2.waitKey(0)
