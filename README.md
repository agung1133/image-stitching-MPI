# Image-Stitching-MPI
sebelum lebih jauh, pastikan MPI telah terinstal pada sistem operasi anda.

<p>lakukan penginstalan numpy, imutils, dan openCV</p>

```
pip install numpy
pip install imutils
pip install open-cv python
```

<p>Buat folder baru didalam shared file untuk menampung gambar yang akan di Stitching</p>

```
mkdir <nama folder>
```
<p>dalam hal ini folder diberi nama "images" </p>

![1700918121723](https://github.com/RyuuAnata/Image-Stitching-MPI/assets/90526948/6e1c2a38-a943-4b81-9c62-4a78ce997ab4)

<p>lalu masukkan gambar yang akan di stitch kedalam folder tersebut</p>

![1700918121713](https://github.com/RyuuAnata/Image-Stitching-MPI/assets/90526948/ae70f8f0-9c44-42b4-869d-0652f839adb3)

<p>sample gambar yang di stitch</p>

![1700884477839](https://github.com/RyuuAnata/Image-Stitching-MPI/assets/90526948/64139cc0-44ac-4787-8e47-0dd994c97ba1)
![1700884477848](https://github.com/RyuuAnata/Image-Stitching-MPI/assets/90526948/3ee129c6-bc13-4f63-8cbe-5b70f7f8ac75)
![1700884477855](https://github.com/RyuuAnata/Image-Stitching-MPI/assets/90526948/3b350406-5292-44f5-8c4c-841bb1dc67ca)

<p>program yang digunakan</p>

```
# USAGE
# python image_stitching_simple.py --images images/scottsdale --output output.png
# import the necessary packages
from imutils import paths
import numpy as np
import argparse
import imutils
import cv2

# construct the argument parser and parse the arguments
ap = argparse.ArgumentParser()
ap.add_argument("-i", "--images", type=str, required=True,
	help="path to input directory of images to stitch")
ap.add_argument("-o", "--output", type=str, required=True,
	help="path to the output image")
args = vars(ap.parse_args())

# grab the paths to the input images and initialize our images list
print("[INFO] loading images...")
imagePaths = sorted(list(paths.list_images(args["images"])))
images = []

# loop over the image paths, load each one, and add them to our
# images to stich list
for imagePath in imagePaths:
	image = cv2.imread(imagePath)
	images.append(image)

# initialize OpenCV's image sticher object and then perform the image
# stitching
print("[INFO] stitching images...")
stitcher = cv2.createStitcher() if imutils.is_cv3() else cv2.Stitcher_create()
(status, stitched) = stitcher.stitch(images)

# if the status is '0', then OpenCV successfully performed image
# stitching
if status == 0:
	# write the output stitched image to disk
	cv2.imwrite(args["output"], stitched)
```

<p>Jalankan perintah</p>

```
mpiexec -n <jumlah slave> -host <nama host> python3 /home/tester/pp/ImageStitching.py -i /home/tester/pp/images -o output.png
```

<p><b>HASIL OUTPUT</b></p>

![output](https://github.com/RyuuAnata/Image-Stitching-MPI/assets/90526948/5022cba1-34e5-465c-9818-5dee6f43f0c8)
