# sides-to-angle
sides ratio to angle

measured all the sides ratios, 

``` python
import cv2 as cv
import apriltag
from google.colab.patches import cv2_imshow as cv_im
import math

options = apriltag.DetectorOptions(families="tag36h11")
detector = apriltag.Detector(options)

r_h = 0
r_w = 0

def pipe(src):
  global r_h, r_w
  # gray
  image = cv.imread(src)
  gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY)

  # resizing
  h, w = image.shape[:2]
  h,w = int(h/3.5),int(w/3.5)
  r_h,r_w = h,w
  resize = cv.resize(gray, (w,h))
  #cv_im(resize) # shows image, good for debug
  return resize

cors = []

#image = pipe(f'/content/20240131_205203.jpg')
#results = detector.detect(image)
#print(results)
#print("corners for tag",results[0].tag_id,results[0].corners)

r = []
angs = []

for i in range(17):
  ang = (i+1)*10
  image = pipe(f'/content/p{ang}.jpg')
  results = detector.detect(image)

  for i in range(len(results)):
    #print(results[i].tag_id,results[i].center)
    print(results[i].tag_id,results[i].corners)
    #cors.append(results[i].corners)
    p1 = results[i].corners[0]
    p2 = results[i].corners[3] # side 1 is abs(p2-p1)
    p3 = results[i].corners[1] # side 2 is abs(p4-p3)
    p4 = results[i].corners[2]

    s1 = abs(p2[1]-p1[1])
    s2 = abs(p4[1]-p3[1])

    val = s1/s2

    r.append(val)
    angs.append(ang)


```

equation `y = -255.5*x+350.7`
x -> side ratios
y -> angle
