## Prototype Development for Image Captioning Using the BLIP Model and Gradio Framework

### AIM:
To design and deploy a prototype application for image captioning by utilizing the BLIP image-captioning model and integrating it with the Gradio UI framework for user interaction and evaluation.

### PROBLEM STATEMENT:
We need to convert the image format and then clean up the model's messy JSON output to show only the simple caption.

### DESIGN STEPS:

#### STEP 1:
Load all required libraries (like os, base64, PIL, requests) and retrieve the Hugging Face API key from the environment variables.

#### STEP 2:
We create a helper function (get_completion) that knows how to talk to the model's API.

#### STEP 3:
We create another helper function (image_to_base64_str) to convert the image from the app (PIL format) into the text string (base64 format) that the API needs.

#### STEP 4:
We define the main captioner function that uses these helpers. It takes the image, converts it (using step 3), sends it to the API (using step 2), and pulls just the caption text out of the JSON response.

#### STEP 5:
Finally, we build the Gradio web app, telling it to use our captioner function whenever a user uploads an image and to display the resulting text in a textbox.

### PROGRAM:
```
import os
import io
import IPython.display
from PIL import Image
import base64

image_url = "https://free-images.com/sm/176d/squirrel_tail_bushy_tail.jpg"
display(IPython.display.Image(url=image_url))
get_completion(image_url)

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
demo = gr.Interface(fn=captioner,
                    inputs=[gr.Image(label="Upload image", type="pil")],
                    outputs=[gr.Textbox(label="Caption")],
                    title="Image Captioning with BLIP",
                    description="Caption any image using the BLIP model",
                    allow_flagging="never",
                    examples=["bmw.jpeg", "cardrift.jpeg", "XUV.jpeg"])

demo.launch(share=True, server_port=int(os.environ['PORT1']))
```
### OUTPUT:

<img width="1172" height="471" alt="Screenshot 2025-11-14 114239" src="https://github.com/user-attachments/assets/a8c6a011-5237-4fc4-9533-4cb331f01be8" />



### RESULT:
Thus, the attempt to deploy a prototype application for image captioning by utilizing the BLIP image-captioning model and integrating it with the Gradio UI framework for user interaction and evaluation is implemented successfully.
