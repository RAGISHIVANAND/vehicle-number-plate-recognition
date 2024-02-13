Steps involved in License Plate Recognition
1. License Plate Detection: The first step is to detect the License plate from the car. We will use the contour option in OpenCV to detect for rectangular objects to find the number plate. The accuracy can be improved if we know the exact size, color and approximate location of the number plate. Normally the detection algorithm is trained based on the position of camera and type of number plate used in that particular country. This gets trickier if the image does not even have a car, in this case we will an additional step to detect the car and then the license plate.

2. Character Segmentation: Once we have detected the License Plate we have to crop it out and save it as a new image. Again this can be done easily using OpenCV.

3. Character Recognition: Now, the new image that we obtained in the previous step is sure to have some characters (Numbers/Alphabets) written on it. So, we can perform OCR (Optical Character Recognition) on it to detect the number

#1.License_Plate_Detection
Let’s take a sample image of a car and start with detecting the License Plate on that car. We will then use the same image for Character Segmentation and Character Recognition as well.

![11111111](https://github.com/RAGISHIVANAND/vehicle-number-plate-recognition/assets/126608984/b3b6a686-d1a6-4a64-95bc-997594dc04ab)

Step 1: Resize the image to the required size and then grayscale it.

The code for the same is given below

img = cv2.imread('/content/N245.jpeg')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
plt.imshow(cv2.cvtColor(gray, cv2.COLOR_BGR2RGB))


Once the counters have been detected we sort them from big to small and consider only the first 10 results ignoring the others. In our image the counter could be anything that has a closed surface but of all the obtained results the license plate number will also be there since it is also a closed surface.

To filter the license plate image among the obtained results, we will loop though all the results and check which has a rectangle shape contour with four sides and closed figure. Since a license plate would definitely be a rectangle four sided figure.


location = None
for contour in contours:
    approx = cv2.approxPolyDP(contour, 10, True)
    if len(approx) == 4:
        location = approx
        break


Once we have found the right counter we save it in a variable called screenCnt and then draw a rectangle box around it to make sure we have detected the license plate correctly.

Now that we know where the number plate is, the remaining information is pretty much useless for us. So we can proceed with masking the entire picture except for the place where the number plate


mask = np.zeros(gray.shape, np.uint8)
new_image = cv2.drawContours(mask, [location], 0,255, -1)
new_image = cv2.bitwise_and(img, img, mask=mask)

The masked new image will appear something like below

![3333333](https://github.com/RAGISHIVANAND/vehicle-number-plate-recognition/assets/126608984/24657ea9-5661-465e-bd10-9782f8d7e64a)


        


Resizing will help us to avoid any problems with bigger resolution images, make sure the number plate still remains in the frame after resizing. Gray scaling is common in all image processing steps. This speeds up other following process sine we no longer have to deal with the color details when processing an image. The image would be transformed something like this when this step is done

![11111111](https://github.com/RAGISHIVANAND/vehicle-number-plate-recognition/assets/126608984/b3b6a686-d1a6-4a64-95bc-997594dc04ab)

Step 2: Every image will have useful and useless information, in this case for us only the license plate is the useful information the rest are pretty much useless for our program. This useless information is called noise. Normally using a bilateral filter (Blurring) will remove the unwanted details from an image. The code for the same is blurred


bfilter = cv2.bilateralFilter(gray, 11, 17, 17) #Noise reduction
edged = cv2.Canny(bfilter, 30, 200) #Edge detection
plt.imshow(cv2.cvtColor(edged, cv2.COLOR_BGR2RGB))


Syntax is destination_image = cv2.bilateralFilter(source_image, diameter of pixel, sigmaColor, sigmaSpace). You can increase the sigma color and sigma space from 15 to higher values to blur out more background information, but be careful that the useful part does not get blurred. The output image is shown below, as you can see the background details (tree and building) are blurred in this image. This way we can avoid the program from concentrating on these regions later.

The next step is interesting where we perform edge detection. There are many ways to do it, the most easy and popular way is to use the canny edge method from OpenCV. The line to do the same is shown below

The syntax will be destination_image = cv2.Canny(source_image, thresholdValue 1, thresholdValue 2). The Threshold Vale 1 and Threshold Value 2 are the minimum and maximum threshold values. Only the edges that have an intensity gradient more than the minimum threshold value and less than the maximum threshold value will be displayed. The resulting image is shown below

![22222222](https://github.com/RAGISHIVANAND/vehicle-number-plate-recognition/assets/126608984/5760025c-214f-4a8d-9978-1a781690a99f)

Step 3: Now we can start Find Contours and Apply Mask

keypoints = cv2.findContours(edged.copy(), cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
contours = imutils.grab_contours(keypoints)
contours = sorted(contours, key=cv2.contourArea, reverse=True)[:10]






Circuit Digest Article:[ https://circuitdigest.com/microcontro...](https://circuitdigest.com/microcontroller-projects/license-plate-recognition-using-raspberry-pi-and-opencv)


OpenCV Documentation: https://docs.opencv.org/4.5.0/

EasyOCR: https://github.com/JaidedAI/EasyOCR


PyTorch: https://pytorch.org/get-started/locally/
