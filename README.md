# Automatic-Number-Plate-Recognition

## ANPR with EasyOCR and pytesseract

### Prerequisite
- Install Pytorch from the link https://pytorch.org/get-started/locally/ with command: **pip3 install torch torchvision torchaudio**
- Installing EasyOCR: **pip install easyocr**
- Installing imutils: **pip install imutils**
- Installing pytesseract: **pip install pytesseract** (Before installing pytesseract, tesseract should be installed in the system and added to the path)

### Steps followed to achieve ANPR
- Install and Import dependencies
  - Install all the libraries mentioned above and import other libraries numpy, torch, imutils, cv2, matplotlib and pytesseract
- Image processing
  - Read the image with cv2
  - Convert in grayscale and blur the image using **bilateralFilter** which is used for edge preseving smoothing, it blurs the image keeping the edge sharp
  - Use **Canny** for edge detection, Canny Edge Detection works as:
    - Noise Reduction: The image is first smoothed using a Gaussian filter to reduce noise and prevent false edge detection.
    -  Gradient Calculation: The gradient intensity of each pixel is calculated using techniques like Sobel filters. The gradient represents the change in intensity, and areas with high intensity change are likely to be edges.
    - Non-Maximum Suppression: This step refines the edges by suppressing pixels that are not at the edge of an object.

    - Double Thresholding: Here, two thresholds (30 and 200 in your case) are applied to determine which edges are strong, weak, or not edges at all:

        - here in this 30 (Lower threshold) is the lower boundary for the gradient intensity. Any edge with a gradient intensity lower than this value is discarded as not being an edge and 200 (Upper threshold) is the upper boundary for the gradient intensity. Any edge with a gradient intensity higher than this value is considered a sure edge.
        - Strong edges are those with a gradient intensity greater than the upper threshold.
        - Weak edges are those with a gradient intensity between the lower and upper thresholds.
        - n-relevant edges are those with a gradient intensity below the lower threshold.
    - Edge Tracking by Hysteresis: The algorithm then links weak edges to strong edges. If a weak edge is connected to a strong edge, it is considered an edge. Otherwise, it is discarded.
  - Find Contours and Apply mask
    - Get the Contours with **findContours** function, Extract the contours directly using imutil's **grab_contours** function and finally take the top 10 contours sorted through calculating area
    - use **approxPolyDP** function approximates a contour to a polygon with fewer vertices, get the contour with 4 points
    - To mask the image:
      - Create an blank black image of the same size as of image and draw a white portion corresponding to the number plate position using the detected contours
      - Finally perform the bitwise AND operation with the original images using mask
  - Crop out the number plate portion from the grayscale image using the masked image
- Read the number from cropped image using EasyOCR and display the detected number on the image
- Perform the ANPR using pytesseract on grascale cropped image and also the black and white image.

### Troubleshooting
- Faced problem while importing the library th pytorch, steps followed to resolve the issue:
  - create a new environment with python 3.9 using command "conda create -n number_plate_rec python=3.9"
  - Activate the environment using "conda activate ocr-env"
  - Install pytorch using command "conda install pytorch torchvision torchaudio cpuonly -c pytorch"
  - Install easyOCR using command "pip install easyocr"
  - Install ipykernel in the Environment using command "pip install ipykernel"
  - Add the Environment to Jupyter using command "python -m ipykernel install --user --name=number_plate_rec"
  - lauch jupyter notebook and go to the change kernel option in the Kernel menu, Select the kernel corresponding to the created kernel
- In some case the same above issue comes up as 'OSError: [WinError 5] Access is denied', and can be sorted using the command: pip install easyocr --user. But for me only above steps worked. 
 
### Future Work
- The number detected here is not exactly proper in some cases, work to enhance the recognition</br></br>


Reference:
1. Python ANPR with OpenCV and EasyOCR in 25 Minutes | Automatic Number Plate Recognition Tutorial, Nicholas Renotte, https://www.youtube.com/watch?v=NApYP_5wlKY


