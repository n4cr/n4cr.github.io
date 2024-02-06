---
title: "Building LLM Powered Software"
date: 2024-02-05T11:14:23+01:00
draft: false
---
There is a lot of hype around AI and Large Language Models. At one side some argue this is yet another wave and will die out. On the other side, some believe that AGI is here and we’re doomed. But the magic is happening somewhere in between.

We already see that in certain industries LLM tools have gone mainstream. A lot of software engineers already use ChatGPT and Github Copilot as coding assistant and this has brought them huge productivity gains. The same ideas are now flowing into many other industries and new products are popping up every day.

From a practical perspective, it is yet not very straightforward to build LLM powered applications. If you’re new to the space you may get overwhelmed by jargons such as RAG, embeddings, chaining, hallucinations etc. Also, when building LLM apps, you’re moving away from building deterministic software to probabilistic software. Engineers are not used to that and LLMs famously make up random stuff due to their probabilistic architecture.

At its core, an LLM is a text engine. You send text in (prompt) and get text back (output). The common use case is building conversational and chat apps. These apps have been largely built and deployed in 2023. However, a lot more can be done with LLMs. Due to their natural language processing capabilities, LLMs can be used to automate or enance processess that are usually done by humans.

The challenges in dealing with prompts and outputs in a programmatic way has lead to a new class of libraries and frameworks. Various frameworks such as Langchain and LammaIndex have gained popularity to streamline building LLM software and bringing structure in it. They offer a full suite of tools to build, test and deploy LLM applications. Also, the documentation pages include many examples and tutorials to get started with.

However, these frameworks introduce a new set of abstraction and complexity on top of the model and create a new learning curve. And when it comes to more custom solutions the code can quickly become complex and unintuitive.


## Thinking about LLM apps intuitively


Building an LLM should not be too different from building classic software. At the end of the day, an LLM is an API that receives text and spits text. If we can get the model to provide us structured data like JSON, then we can build application the same way we build classic software. The problem is that receiving structured data from the LLM is not straightforward. You may even have to beg or threaten the model to give you JSON (literally) and yet you may not receive what you asked for.

Last year, OpenAI introduced function calling to their completion API which allows the user to add tools to the model. Using tools, you can tell the model to return the signature and arguments for a function call instead of text, then you can use that info to call the function in your code.

For example, you can tell the LLM model that you have a "get_weather(city)"  function which calls a third party weather API. If you ask "How’s the weather in Amsterdam?" Then you’ll get back the equivalent of "call the function get_weather with the argument Amsterdam" from the model. Once you receive that, you call the function, get the response in JSON something like:

```json
{
    "city": "amsterdam",
    "temperature": "5c", 
    "wind": "10km East"
} 
```


Then, you send the response back to the model and get a nicely written text like this:
The weather in amsterdam is 5 degrees celcius with 10KM winds towards east.

This is quite useful as you can connect the LLM to other tools and interfaces. But more can be done with this functionality.

What’s interesting here is that OpenAI API functions are defined using standard JSON schema. For example, here is function definition:

```json
    {
        "type": "function",
        "function": {
            "name": "get_current_weather",
            "description": "Get the current weather",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "The city and state, e.g. San Francisco, CA",
                    },
                    "format": {
                        "type": "string",
                        "enum": ["celsius", "fahrenheit"],
                        "description": "The temperature unit to use. Infer this from the users location.",
                    },
                },
                "required": ["location", "format"],
            },
        }
    }
```


The function definition is standard JSON schema and can be easily mapped to a data class or Pydantic model. For example I can have a model