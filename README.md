TITLE: Face Recognition System

Step 1: Setting Up Google Teachable Machine

Go to the Google Teachable Machine website.

![WhatsApp Image 2025-01-15 at 09 57 52_8978c4a9](https://github.com/user-attachments/assets/e1fe6ee6-849c-415e-aa5c-5ab9cd5bb62b)

Select the Image Project option

![WhatsApp Image 2025-01-15 at 09 58 37_a8421c39](https://github.com/user-attachments/assets/0abf96d8-59b5-4474-ac40-551723680d46)

Choose the Standard Image Model or Face Recognition (depending on your goal).

Create classes by capturing images of faces for each class (e.g., different individuals).

![WhatsApp Image 2025-01-15 at 09 59 26_4682f399](https://github.com/user-attachments/assets/6df6a5e2-b597-4c46-a6c5-0068d8b88655)
![Screenshot 2025-01-14 212320](https://github.com/user-attachments/assets/22d59edc-4aba-45ff-8730-b7c31dbca857)


Step 2: Exporting the Trained Model

After training, click on the Export Model button.

![WhatsApp Image 2025-01-15 at 10 02 14_ac0ce7ee](https://github.com/user-attachments/assets/66f09ed4-4af1-4e06-9018-321aa77cd41a)

Select the TensorFlow option.

Download the Keras .h5 model file by clicking on the appropriate export link.

![WhatsApp Image 2025-01-15 at 10 02 52_3d988518](https://github.com/user-attachments/assets/102a1b21-1067-4b1c-b14f-4389cebaf208)

Step 3: Setting Up the Python Environment

Install Python (if not already installed) by downloading it from python.org. Ensure Python 3.7+ is installed.

Install a code editor like VS Code or PyCharm for coding.

Open a terminal and create a virtual environment:

python -m venv face_rec_env

Activate the virtual environment: On Windows:

face_rec_env\Scripts\activate On Mac/Linux:

source face_rec_env/bin/activate

Step 4: Installing Required Libraries

Run the following commands in the terminal to install the necessary libraries:

pip install tensorflow pip install numpy pip install opencv-python pip install matplotlib pip install pillow

Step 5: Loading the Exported Model

Place the .h5 model file downloaded from Teachable Machine in your project directory. Create a Python script (e.g., face_recognition.py) and add the following code to load the model: python

import tensorflow as tf

Load the Teachable Machine Keras model
model = tf.keras.models.load_model('model.h5') print("Model loaded successfully!")

Step 6: Capturing and Preprocessing Images

Use OpenCV to capture images from your webcam: python

import cv2

Open the webcam
cap = cv2.VideoCapture(0)

while True: ret, frame = cap.read() cv2.imshow('Webcam', frame)

# Press 'q' to exit
if cv2.waitKey(1) & 0xFF == ord('q'):
    break
cap.release() cv2.destroyAllWindows() Preprocess the images to match the input shape of the Teachable Machine model. Typically, resize them to the required size (e.g., 224x224 pixels). Description of step6: This code snippet uses the OpenCV library to capture video from a webcam and display it in real-time. Below is a detailed explanation of each part of the code:

Code Breakdown and Description

Import OpenCV Library
import cv2 The cv2 module is imported to use OpenCV, a library for computer vision tasks such as image processing, video capture, and manipulation.

Open the Webcam
cap = cv2.VideoCapture(0) cv2.VideoCapture(0): Opens the default webcam (device index 0). If you have multiple cameras, you can change the index (e.g., 1 for a second camera). The cap object is used to interact with the webcam for capturing video frames.

Infinite Loop to Capture Frames
while True: ret, frame = cap.read() cap.read(): Captures the current frame from the webcam. ret: A boolean value indicating whether the frame was successfully captured (True or False). frame: The actual frame captured, represented as a NumPy array. The loop ensures continuous video capture until stopped.

Display the Webcam Feed
cv2.imshow('Webcam', frame) cv2.imshow(): Displays the captured frame in a window titled "Webcam". The frame is shown in real-time as a video stream.

Exit Condition
if cv2.waitKey(1) & 0xFF == ord('q'): break cv2.waitKey(1): Waits for 1 millisecond for a key press. If the user presses the 'q' key, the condition becomes True, and the loop exits. The ord('q') converts the character 'q' into its ASCII code for comparison.

Release Resources
cap.release() cv2.destroyAllWindows() cap.release(): Releases the webcam resource so it can be used by other applications. cv2.destroyAllWindows(): Closes all OpenCV windows that were created during the execution of the program. What the Code Does Opens the default webcam and starts capturing video frames. Displays the video stream in real-time in a window named "Webcam". Allows the user to press the 'q' key to stop the video and close the window. Potential Applications Testing Webcam Functionality: Check if the webcam is working. Base Code for Vision Projects: This is a foundational script for real-time video processing, such as object detection or face recognition.

Step 7: Making Predictions

Add the following code to preprocess the image and make predictions:

python code for face recognition: from keras.models import load_model import cv2 import numpy as np

Disable scientific notation for clarity
np.set_printoptions(suppress=True)

Load the model
model = load_model("keras_Model.h5", compile=False)

Load the labels
class_names = open("labels.txt", "r").readlines()

Try different backends (CAP_MSMF or CAP_DSHOW) if the default fails
camera = cv2.VideoCapture(0, cv2.CAP_DSHOW)

if not camera.isOpened(): print("Error: Could not open the camera.") exit()

while True: # Grab the webcam's image ret, image = camera.read()

if not ret:
    print("Error: Failed to capture image from the camera.")
    break

# Resize the raw image into (224-height,224-width) pixels
try:
    image_resized = cv2.resize(image, (224, 224), interpolation=cv2.INTER_AREA)
except cv2.error as e:
    print(f"OpenCV Error: {e}")
    break

# Show the image in a window
cv2.imshow("Webcam Image", image)

# Make the image a numpy array and reshape it to the model's input shape
image_array = np.asarray(image_resized, dtype=np.float32).reshape(1, 224, 224, 3)

# Normalize the image array
image_array = (image_array / 127.5) - 1

# Predict using the model
prediction = model.predict(image_array)
index = np.argmax(prediction)
class_name = class_names[index].strip()
confidence_score = prediction[0][index]

# Print prediction and confidence score
print(f"Class: {class_name}, Confidence Score: {np.round(confidence_score * 100, 2)}%")

# Listen to the keyboard for presses
keyboard_input = cv2.waitKey(1)

# 27 is the ASCII for the ESC key on your keyboard
if keyboard_input == 27:
    print("Exiting program.")
    break
camera.release() cv2.destroyAllWindows()

DESCRIPTION OF ABOVE CODE:

Imports Libraries: Loads necessary libraries (keras, cv2, numpy) for machine learning and computer vision tasks.

Load Model and Labels:

Loads a pre-trained Keras model from keras_Model.h5. Reads class labels from a text file (labels.txt). Initialize Webcam: Opens the default webcam using OpenCV, with error handling if the webcam fails to initialize.

Real-Time Image Capture:

Continuously captures video frames from the webcam. Resizes each frame to (224, 224) pixels to match the model’s input size. Normalizes pixel values to the range [-1, 1]. Model Prediction:

Processes the resized frame through the model to generate predictions. Finds the class with the highest confidence (argmax). Displays the predicted class and confidence score in the console. Display Webcam Feed: Shows the live webcam feed in a window.

Exit Mechanism:

Pressing the ESC key (ASCII 27) stops the loop and closes the webcam feed. Clean Up:

Releases the webcam and closes all OpenCV windows when exiting.

Creating a face recognition system using Google Teachable Machine and exporting it to a Keras model involves the following steps. Follow these instructions step by step:

Step 1: Setting Up Google Teachable Machine Go to the Google Teachable Machine website. Select the Image Project option. Choose the Standard Image Model or Face Recognition (depending on your goal). Create classes by capturing images of faces for each class (e.g., different individuals). Train the model by clicking the Train Model button.

Step 2: Exporting the Trained Model

After training, click on the Export Model button. Select the TensorFlow option. Download the Keras .h5 model file by clicking on the appropriate export link.

Step 3: Setting Up the Python Environment

Install Python (if not already installed) by downloading it from python.org. Ensure Python 3.7+ is installed. Install a code editor like VS Code or PyCharm for coding. Open a terminal and create a virtual environment:

python -m venv face_rec_env Activate the virtual environment: On Windows:

face_rec_env\Scripts\activate On Mac/Linux:

source face_rec_env/bin/activate

Step 4: Installing Required Libraries

Run the following commands in the terminal to install the necessary libraries: pip install tensorflow pip install numpy pip install opencv-python pip install matplotlib pip install pillow

Step 5: Loading the Exported Model

Place the .h5 model file downloaded from Teachable Machine in your project directory. Create a Python script (e.g., face_recognition.py) and add the following code to load the model:

import tensorflow as tf

Load the Teachable Machine Keras model
model = tf.keras.models.load_model('model.h5') print("Model loaded successfully!")

Step 6: Capturing and Preprocessing Images

Use OpenCV to capture images from your webcam:

import cv2

Open the webcam
cap = cv2.VideoCapture(0)

while True: ret, frame = cap.read() cv2.imshow('Webcam', frame)

# Press 'q' to exit
if cv2.waitKey(1) & 0xFF == ord('q'):
    break
cap.release() cv2.destroyAllWindows() Preprocess the images to match the input shape of the Teachable Machine model. Typically, resize them to the required size (e.g., 224x224 pixels).

Step 7: Making Predictions

Add the following code to preprocess the image and make predictions:

import numpy as np from PIL import Image

def preprocess_image(image): # Resize the image to match model input img = Image.fromarray(image) img = img.resize((224, 224)) # Replace with the size your model expects img_array = np.array(img) / 255.0 return np.expand_dims(img_array, axis=0)

while True: ret, frame = cap.read() if not ret: break

# Preprocess the frame
processed_frame = preprocess_image(frame)

# Make prediction
predictions = model.predict(processed_frame)
class_idx = np.argmax(predictions)
confidence = predictions[0][class_idx]

# Display prediction
cv2.putText(frame, f"Class: {class_idx}, Confidence: {confidence:.2f}", 
            (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 255, 0), 2)
cv2.imshow('Webcam', frame)

if cv2.waitKey(1) & 0xFF == ord('q'):
    break
cap.release() cv2.destroyAllWindows() Step 8: Testing and Finalizing: run the code

output:
![Screenshot 2025-01-15 100611](https://github.com/user-attachments/assets/bede2679-0a84-446a-a8d5-cb2fba7747ac)
