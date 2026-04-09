# Beam Drift Camera and alignment actuators

### Goals:
The goal of this project is to have Cameras that sample the beams and monitor there alignment to the ion position. 
And to have actuators on the alignment mirror nobs to align the beam with a feedback loop to the camera. 

### Initial Test with the spare Laser:
I built a system using a Raspberry Pi and a USB camera to track the position of a laser beam over time.
Image acquisition and processing is done with OpenCV
```python
cap = cv2.VideoCapture("/dev/video0", cv2.CAP_V4L2)
```
The stream runs continuously at 20FPS. 
The images are converted to grayscale and smoothed out with Gaussian blur for noise reduction. 
```python
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
blur = cv2.GaussianBlur(gray, (21, 21), 0)
```
I isolate the beam by thresholding the brightest region, this way i can keep track of peak brightness (in arb. units) and detection will adapt to changes in brightness:
```python
peak = blur.max()
threshold = max(30, int(0.5 * peak))
_, mask = cv2.threshold(blur, threshold, 255, cv2.THRESH_BINARY)
```
ChatGPT recommended I use morphological filtering:
```python
mask = cv2.morphologyEx(mask, cv2.MORPH_OPEN, kernel)
mask = cv2.morphologyEx(mask, cv2.MORPH_CLOSE, kernel)
```
I compute the beam position using an intensity-weighted centroid:
```python
ys, xs = np.nonzero(mask)
weights = blur[ys, xs]

cx = sum(xs * weights) / sum(weights)
cy = sum(ys * weights) / sum(weights)
```
I then calculate the drift from the initial position of the beam and log that in graphana along with some other metrics. This is what the stream looks like and the Graphana position tracking and power tracking:
![[Screenshot 2026-03-29 163216.png|529]]

In the stream image blue crosshair show the center of the censor
green dot shows the center of the beam. Red crosshair shows the initial position of the beam. 
I display various live values along with filtered mask used to calculate the beam center position. There are some dark spots on the camera censor, but that's fine for now.

![[Pasted image 20260329164912.png]]

In the Laser position drift plot the current position is displayed in red. 
