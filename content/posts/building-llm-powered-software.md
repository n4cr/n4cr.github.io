---
title: "Building LLM Powered Software"
date: 2024-02-05T11:14:23+01:00
draft: false
---
There is a lot of hype around AI and Large Language Models. On one side of the spectrum some argue that this is another wave and will die out. On the other side, some believe that AGI is here and we’re doomed. The magic is happening somewhere in the middle.

We already see that in certain industries LLM tools have gone mainstream. A lot of software engineers already use ChatGPT and Github Copilot as coding assistant and this has brought huge productivity gains. The same ideas are now flowing into many other industries and many products are being built in this space.

From a practical perspective, it is yet not very straightforward to build LLM powered applications. If you’re new to the space you will be hearing lots of jargons such as RAG, embeddings, chaining, hallucinations etc. Also, when building LLM apps, you’re moving away from building deterministic software to probabilistic software. LLMs famously make up random stuff due to their probabilistic architecture.

They are interfaced in text. Meaning if you want to build an LLM powered app, you’d need to render and send prompts with context and variables, parse the output and use in your apps.

This is not a very intuitive way of building software and requires a change in perspective. Various frameworks (e.g. Langchain, LammaIndex) have gained popularity to streamline building LLM software and bring structure in it. They are easy to get started with and quite capable. However, the code becomes pretty complex in no time and debugging is difficult.


## Thinking about LLM apps intuitively
Building an LLM should not be too different from building any classic software. At the end of the day, an LLM is an API that receives text and spits text. If we can get the model to give us structured data like JSON, then we can build application the same way we build classic software. The problem is that receiving structured data from the LLM is not straightforward. You may even have too beg or threaten the model to give you JSON (literally) and yet you may not receive what you asked for.

Last year, OpenAI introduced function calling to their completion API which allows the user to add tools to the model. Using tools, you can tell the model to return the signature and arguments for a function call instead of text, then you can use that info to call the function in your code.

For example, you can tell the LLM model that you have a “get_weather(city)”  function which calls a third party weather API. If you ask “How’s the weather in Amsterdam?” Then you’ll get back the equivalent of “call the function get_weather with the argument Amsterdam” from the model. Once you receive that, you call the function, get the response in JSON something like:
