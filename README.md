Steps involved in License Plate Recognition
1. License Plate Detection: The first step is to detect the License plate from the car. We will use the contour option in OpenCV to detect for rectangular objects to find the number plate. The accuracy can be improved if we know the exact size, color and approximate location of the number plate. Normally the detection algorithm is trained based on the position of camera and type of number plate used in that particular country. This gets trickier if the image does not even have a car, in this case we will an additional step to detect the car and then the license plate.

2. Character Segmentation: Once we have detected the License Plate we have to crop it out and save it as a new image. Again this can be done easily using OpenCV.

3. Character Recognition: Now, the new image that we obtained in the previous step is sure to have some characters (Numbers/Alphabets) written on it. So, we can perform OCR (Optical Character Recognition) on it to detect the number

1. License Plate Detection
Let’s take a sample image of a car and start with detecting the License Plate on that car. We will then use the same image for Character Segmentation and Character Recognition as well.

![11111111](https://github.com/RAGISHIVANAND/vehicle-number-plate-recognition/assets/126608984/b3b6a686-d1a6-4a64-95bc-997594dc04ab)



Circuit Digest Article:[ https://circuitdigest.com/microcontro...](https://circuitdigest.com/microcontroller-projects/license-plate-recognition-using-raspberry-pi-and-opencv)


OpenCV Documentation: https://docs.opencv.org/4.5.0/

EasyOCR: https://github.com/JaidedAI/EasyOCR


PyTorch: https://pytorch.org/get-started/locally/
