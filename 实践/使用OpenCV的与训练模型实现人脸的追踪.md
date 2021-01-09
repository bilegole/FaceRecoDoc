opencv有一些预训练的模型,可以帮助我们圈出人脸框,比如:
~~~python
import cv2
face_Cascade = cv2.CascadeClassifier('haarcassade+frontalface_default.xml')
img = cv2.imread('path_to_image/image.jpg')
gray = cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
faces = face_Cascade.detectMultiScale(gray,1.1,4)
for x,y,w,h in faces:
	cv2.rectangle(img,(x,y),(x+w,y+h),(255,0,0),2)
cv2.imshow('img',img)
cv2.waitKey()
~~~
这个模型可以从此仓库下载:
~~~
github.com/opencv/opencv/tree/master/data/haarcascades
~~~
