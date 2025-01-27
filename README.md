# Instructions to Run

## Requirements

### Visual Studio Code C++ Build Tools:
- Download from [Visual Studio](https://visualstudio.microsoft.com/vs/community/).
- Once the setup wizard is complete, it will open a workloads page.
- Make sure "Desktop Development for C++" is checked, and click download (~1.94 gigabytes).

### Install Protocol Buffers from the Protoc Github:
- Download the x64 version for your operating system from [GitHub](https://github.com/protocolbuffers/protobuf/releases) (e.g., `protoc.{X}.{OS}.zip`).
- Move `protoc.zip` somewhere, extract it, and rename the extracted folder to `protoc`.
- Add `protoc\bin` to PATH (Edit environmental variables, copy the location where you put `protoc` into PATH).
- Your PATH should look something like this: `C:\Additional Programs\protoc\bin`.

### Install TensorFlow Object Detection Repository:
- Open Command Prompt with administrative privileges.
- I recommend creating a virtual environment for the libraries.
- Run the following commands in the file path where you want to run the code:
  ```sh
  git clone https://github.com/tensorflow/models
  cd models
  cd research
  protoc object_detection/protos/*.proto --python_out=.
  copy object_detection\packages\tf2\setup.py .
  python -m pip install .
  ```
- This will install ~50 packages used for TensorFlow Object Detection.
- Any other libraries that may be missed can be installed using the !pip install {library} command.

# Main Idea
In order for these scientists to rapidly count the number of barnacles within these images, fine-tuning a pre-trained model could suffice to estimate the number of barnacles across many of these platforms.

## Critical Subtasks
1. **Resizing Images**:
   - The two given images are of different sizes. Resizing the data into a more easily processed format will make it easier to train my model.
   - I will code a method to receive an input image and resize it to 1280 by 1280 pixels.

2. **Data Augmentation**:
   - There are only two given images. Some sort of data augmentation is necessary to give the model enough data.
   - I will split both images into sixteen 320 by 320 pixel parts each, for thirty-two images total.
   - I will then apply variation by horizontal flips, vertical flips, brightness, and contrast adjustments.

3. **Pascal VOC Formatting**:
   - The mask images are not in standard Pascal VOC formatting. They must be converted into this formatting to be compatible with Tensorflow Zoo models.
   - Images that the model will be trained on will have a corresponding Pascal VOC .xml file created for them.
   - These Pascal VOC .xml files have bounding boxes. If the area of the bounding box for a barnacle is greater than 50% in one of the split images, it stays; otherwise, it is deleted. This keeps mostly cut barnacles from having bounding boxes.

## Building the Prototype
- After the preprocessing tasks shown above, a model will be built starting from the pre-training of the `ssd_mobilenet_v2_coco` model, which is the most lightweight machine learning model.
- I chose this model because of two reasons:
  1. I do not have a GPU on my computer, and a lightweight model will allow me to simultaneously train it while working on work for classes. (I experimented with the hourglass model but it took 80 seconds to train one step and froze my computer).
  2. This model can easily run on phones because of how lightweight it is on live video. This could potentially be useful for these scientists.

## Training the Model
- I trained the model on 20,000 steps.
- I did a train-test split of about 75-25 manually.

## Evaluating the Data
- Because I am using such a lightweight model not well-known for its accuracy, I will be evaluating the data on Mean Absolute Percentage Error (MAPE), which shows the average percentage the model is away from the actual number of barnacles in the image.
- I chose this over Mean Absolute Error because the two images provided have differing numbers of barnacles in them, so percentage would normalize differences between images.
- I did not choose Mean Squared Error because I thought the squared penalty would make understanding the effectiveness of my model less intuitive.

The Mean Absolute Percentage Error (MAPE) I received was 21.790573037649686, which is not too bad considering I used the most lightweight model in all of Tensorflow 2 Model Zoo. The model can be used for rough estimation based on this metric. Furthermore, it can help scientists label the barnacles, by outlining them, to maybe speed up the process.

## Conclusions
- I only had enough time to go through a single run of training, but if I had more, I would likely try color quantizing the image using K-means, as shown in my second notebook. This could help to improve barnacle identification.
- One thing I learned during this process is how to edit .xml files into Pascal VOC formatting which excited me, because I had always used tzutalin’s labelImg to do this prior.
