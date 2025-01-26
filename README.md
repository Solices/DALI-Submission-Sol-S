# Instructions to Run

## Requirements

### Visual Studio Code C++ Build Tools:
- Download from [Visual Studio](https://visualstudio.microsoft.com/vs/community/).
- Once the setup wizard is complete, it will open a workloads page.
- Make sure "Desktop Development for C++" is checked off, and click download (~1.94 gigabytes).

### Install Protocol Buffers using the Protoc Library:
- Download the x64 version for your operating system from [GitHub](https://github.com/protocolbuffers/protobuf/releases) (e.g., `protoc.{X}.{OS}.zip`).
- Move `protoc.zip` somewhere, extract it, and rename the extracted folder to `protoc`.
- Add `protoc\bin` to PATH (Edit environmental variables, copy the location where you put `protoc` into PATH).
- Your PATH should look something like this: `C:\Additional Programs\protoc\bin`.

### Install TensorFlow Object Detection Repository:
- Open Command Prompt with administrative privileges.
- I recommend creating a virtual environment for the libraries.
- Run the following commands:
  ```sh
  git clone https://github.com/tensorflow/models
  cd models
  cd research
  protoc object_detection/protos/*.proto --python_out=.
  copy object_detection/packages/tf2/setup.py .
  python -m pip install .
  ```
- This will install ~50 packages used for TensorFlow Object Detection.
- Any other libraries that may be missed can be installed using the !pip install {library} command.