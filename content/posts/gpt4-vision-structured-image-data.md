---
title: "GPT4 Vision Structured Image Data"
date: 2024-03-05T21:38:01+01:00
draft: false
---

The OpenAI [GPT-4 vision model](https://platform.openai.com/docs/guides/vision) allows you to understand images and generate text about them. This model is used by ChatGPT when you upload an image and ask a question about it. It is quite powerful and can be used in a variety of applications. The model is also accesible via the OpenAI API. You can upload an image and run a prompt against it to generate a text output.

An interesting use case is to extract structured data from images so you can make them searchable. Imagine, you have a bunch of aerial images for some land and you want to have the images indexed structurally in a database. You can use GPT-4 vision to extract a JSON object from the image. Here are two examples and then next is the code to do it.

![Parking Image](/gpt4-vision-image-analysis/img2.jpg)

Here is the output:

```json
{
    "title": "Aerial View of a Parking Lot",
    "description": "The image is an aerial photograph of a parking lot with various parked cars. The cars are arranged in rows with a mix of colors and models. The parking lot surface is a light gray with visible tire marks. The image can be used to analyze parking patterns or for urban planning studies.",
    "features": [
        "parking lot",
        "parked cars",
        "tire marks",
        "aerial view"
    ]
}
```

Another example

![Parking Image](/gpt4-vision-image-analysis/img3.jpg)

Here is the output:

```json
{
    "title": "Aerial View of a Beach Pier",
    "description": "The image is an aerial shot of a sandy beach with a wooden pier extending into the sea. People are visible on the beach and in the water, some swimming and others lounging on beach towels or under umbrellas. The water transitions from a clear turquoise near the sand to a deeper blue further out.",
    "features": [
        "sandy beach",
        "wooden pier",
        "people swimming",
        "people lounging",
        "beach towels",
        "beach umbrellas",
        "turquoise water",
        "deep blue sea"
    ]
}
```

Here is the code that generates these outputs:

```python
from typing import List
import openai
import instructor
from pydantic import BaseModel, Field
import base64
from dotenv import load_dotenv

load_dotenv(dotenv_path="../.env")  # take environment variables from ../.env.

client = instructor.patch(openai.OpenAI(), mode=instructor.Mode.MD_JSON)


class ImageAnalysis(BaseModel):
    title: str = Field(
        ...,
        description="The title of the image.",
    )

    description: str = Field(
        ...,
        description="The description of the image. Include any summary that can help someone find the image in a database.",
    )
    features: List[str] = Field(
        ...,
        description="A list of objects that are present in the image.",
    )


def analyse_image(img) -> ImageAnalysis:
    print("Processing image: ", img)
    with open(img, "rb") as image_file:
        encoded_string = base64.b64encode(image_file.read()).decode("utf-8")
    image_url = f"data:image/jpeg;base64,{encoded_string}"
    resp = client.chat.completions.create(
        model="gpt-4-vision-preview",
        max_tokens=4096,
        max_retries=2,
        response_model=ImageAnalysis,
        temperature=0,
        messages=[
            {
                "role": "system",
                "content": """
                You are a world-class analyst tasked with analyzing aerial photos.
                Your goal is to identify the objects in the image and provide a detailed analysis of the image.
                Include any features that can enhance understanding of the image.""",
            },
            {
                "role": "user",
                "content": [
                    f"Return a detailed analysis of the image, adhering to the structure defined by {ImageAnalysis.model_json_schema()}",
                    *[{"type": "image_url", "image_url": image_url}],
                ],
            },
        ],
    )
    return resp
```