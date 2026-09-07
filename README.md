## Prototype Development for Image Captioning Using the BLIP Model and Gradio Framework

### AIM:
To design and deploy a prototype application for image captioning by utilizing the BLIP image-captioning model and integrating it with the Gradio UI framework for user interaction and evaluation.

### PROBLEM STATEMENT:
Image captioning is a challenging task in Artificial Intelligence that combines Computer Vision and Natural Language Processing to automatically generate meaningful textual descriptions for images. Manual image annotation is time-consuming and impractical for large-scale applications such as social media platforms, digital libraries, and assistive technologies for visually impaired individuals. Therefore, an automated system is required to accurately analyze image content and generate relevant captions in natural language.

The objective of this project is to develop a prototype image captioning application using the BLIP (Bootstrapping Language-Image Pre-training) model integrated with the Gradio framework. The system should accept image inputs from users and automatically generate descriptive captions through an interactive web-based interface.

### DESIGN STEPS:

#### STEP 1:

Install and import the necessary libraries such as transformers, torch, PIL, and gradio required for image processing and caption generation.

#### STEP 2:

Load the pre-trained BLIP image captioning model and processor from the Hugging Face Transformers library.

#### STEP 3:

Create a Python function that accepts an input image, processes it using the BLIP model, and generates an appropriate textual caption.

#### STEP 4:

Design an interactive Gradio user interface that enables users to upload images and view generated captions instantly.

#### STEP 5:

Launch and test the Gradio application locally or through a browser interface to evaluate the performance of the image captioning system.

### PROGRAM:
```PYTHON
import os
import io
import IPython.display
from PIL import Image
import base64 
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']
```
```PYTHON
# Helper functions
import requests, json

#Image-to-text endpoint
def get_completion(inputs, parameters=None, ENDPOINT_URL=os.environ['HF_API_ITT_BASE']):
    headers = {
      "Authorization": f"Bearer {hf_api_key}",
      "Content-Type": "application/json"
    }
    data = { "inputs": inputs }
    if parameters is not None:
        data.update({"parameters": parameters})
    response = requests.request("POST",
                                ENDPOINT_URL,
                                headers=headers,
                                data=json.dumps(data))
    return json.loads(response.content.decode("utf-8"))
```
```PYTHON
image_url = "https://free-images.com/lg/50d5/apple_desk_laptop_macbook.jpg"
display(IPython.display.Image(url=image_url))
get_completion(image_url)
```
```PYTHON
import gradio as gr 

def image_to_base64_str(pil_image):
    byte_arr = io.BytesIO()
    pil_image.save(byte_arr, format='PNG')
    byte_arr = byte_arr.getvalue()
    return str(base64.b64encode(byte_arr).decode('utf-8'))

def captioner(image):
    base64_image = image_to_base64_str(image)
    result = get_completion(base64_image)
    return result[0]['generated_text']

gr.close_all()
print("Name: AMIRTHA VARSHINI M")
print("Register Number: 212224230017")
demo = gr.Interface(fn=captioner,
                    inputs=[gr.Image(label="Upload image", type="pil")],
                    outputs=[gr.Textbox(label="Caption")],
                    title="Image Captioning with BLIP",
                    description="Caption any image using the BLIP model",
                    allow_flagging="never",
                    examples=["christmas_dog.jpeg", "bird_flight.jpeg", "cow.jpeg"])

demo.launch(share=True, server_port=int(os.environ['PORT1']))
```


### OUTPUT:

<img width="1367" height="777" alt="image" src="https://github.com/user-attachments/assets/803b112d-2bba-4cbb-8d8e-ffc823b7f547" />

<img width="1194" height="901" alt="image" src="https://github.com/user-attachments/assets/0467d0c8-fd80-4a6e-ae72-30a895501790" />

### RESULT:

The prototype image captioning application was successfully developed using the BLIP model and Gradio framework. The system effectively generates meaningful captions for uploaded images through an interactive and user-friendly interface.
