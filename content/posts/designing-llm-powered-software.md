---
title: "Desiging LLM Powered Software"
date: 2024-02-05T11:14:23+01:00
draft: false
---
The first wave of LLM apps were famously looked down upon as "gpt wrappers". Single prompt templates generating simple text. But if you're building an AI first application, you need an AI design approach. And that's the moat. You combine the engineering approaches to create a new generation of software. A software that is more natural and more human. Or can process human generated knowledge. 

There is a lot of hype around AI and Large Language Models. At one side some argue this is yet another wave and will die out. On the other side, some believe that AGI is here and we’re doomed. But the magic is happening somewhere in between. We already see that in certain industries LLM tools have gone mainstream. A lot of software engineers already use ChatGPT and Github Copilot as coding assistant and this has brought them huge productivity gains. The same ideas are now flowing into many other industries and new products are popping up every day.

At its core, an LLM is a text engine. You send text in (prompt) and get text back (output). The common use case is building conversational and chat apps. These apps have been largely built and deployed in 2023. However, a lot more can be done with LLMs. Due to their natural language processing capabilities, LLMs can be used to automate or enance processess that are usually done by humans.

From a practical perspective, it is yet not very straightforward to build LLM powered applications. If you’re new to the space you may get overwhelmed by jargons such as RAG, embeddings, chaining, hallucinations etc. Also, when building LLM apps, you’re moving away from building deterministic software to probabilistic software. Engineers are not used to that and LLMs famously make up random stuff due to their probabilistic architecture.

Another challenge is handling prompts and outputs effectively. It's common that you need to render a prompt with variable coming from the web app, execute against the LLM and parse the output. And you need to do it with natural langugage. It can and will go wrong often due to probabilistic nature of the model. This has led to the emergence of wrapper frameworks such as Langchain. They come with a lot of features and many examples. But they also add another layer of abstraction and complexity to the underlying models, introducing a steep learning curve. Also, developing  productoin grade solutions can result in code that is both complex and difficult to understand.

I will try to expland some of these concepts and provide some practical tips on how to build LLM powered software.

## To Stream or Not to Stream

A big question when designing an AI system is whether to stream the output or not. This is a question often overlooked but can have a big impact on the user experience and the performance of the system.

### Streaming
Streaming data to the user as soon as it is available, rather than waiting for the entire response to be ready is generally nice. Though it is not useful in all cases. Consider the following example: 


<button onclick="fetchData(this)" data-target="#output" data-t="
    Write a story about a dog.
    ">Write a story about a dog</button>
<div id="output" class="prompt"></div>

Here, the streaming is a natural fit. The user does not need to wait. You see the story as it is being written, it is fast and interactive. And it is generated content. Also, in this case the output is not too large. So streaming it it is overall a good idea.


Now try this one:

<button onclick="fetchData(this)" data-target="#slow" data-s="1" data-t="Write a story about a cat.">Write a story about a cat</button>
<div id="slow" class="prompt"></div>

No one wants to sit and wait for the story to be written. Especially if it is a long story. Here I've exaggerated and have slowed it down. But it is not uncommon to have a slow generated response. Especially when you need to use slower models such as GPT-4. 

Another downside of streaming is error handling. If the stream is interrupted, then it has be run again. If you're generating a long blog post and the browser tab is closed, then you have to start over.

My general rule of thumb is to stream the output only when generating content (e.g. blog post, story, code) and instant feedback is needed. Onboarding, chat and other conversational apps are good examples. 


> **BIG TIP:** You don't need a websocket server to handle streaming. 
> You can stream from the server in an open HTTP connection.
> I've [answered](https://stackoverflow.com/a/77950664/446338) a StackOverflow question on this if you want to know how (who usess SO these days?!).

### Short Wait Times

When it comes to structured data, always wait for the entire response to be ready. The partially streamed data is useless anyway. This is especially true when the response is longer and the user needs to see the data as soon as possible.

<button onclick="fetchData(this)" data-target="#weather" data-t="Generate a json object for weather in Amsterdam.">Write a weather json</button>
<div id="weather" class="prompt"></div>

Instead, you its not a big penalty to wait for the entire response to be ready. And it can perfectly fit the user's expectation to wait a bit for the data to be ready. Add to it a spinner or progress bar and you've got a nice setup.

<button hx-get="https://playground.semicolon.io/events" hx-vals='{"t":"Generate a json object for weather in Amsterdam."}' hx-target="#weather2" hx-trigger="click" hx-indicator="#spinner">Write a weather json</button>
<div id="spinner" class="spinner">

</div>
<div id="weather2" class="prompt"></div>

There are lots of benefits to using structured data. I will write about it in the future. Short version is that, using structured data makes development more natural and closer to what we're used to.


### Long Wait Times

When it comes to a lot larger data, or long processes it's better to change approach entirely and use background workers. Many webservers already timeout after 30 seconds. So you can simply follow the standard approach and use a background worker to generate the content and notify the user when it's ready using a status field. This is a more complex approach but it's the best way to handle large data.



# Combine Them All

Combining these approaches allows you to build a more versatile and user friendly applications. Depending on the user flows, you can use different approaches to make it as nautral and/or/xor functional as possible.

If you think about it, this is not much different from how we build classic software. The magic is happenning in the reasoning and natural language capabilities. What matters is the flows and the user experience. And that's where the moat is.


<script>
    
    async function fetchData(button) {
    
    button.disabled = true;
    const t = button.getAttribute('data-t');
    const s = button.getAttribute('data-s') || '';
    const targetSelector = button.getAttribute('data-target');
    const target = document.querySelector(targetSelector);
    target.innerHTML = '';

    const url = `https://playground.semicolon.io/events?s=${s}&t=${t}`; // Replace with your URL
    
    const response = await fetch(url)
    
    const reader = response.body.getReader()
    const decoder = new TextDecoder()
    while (true) {
        const { value, done } = await reader.read()
        if (done) {
        break
        }
        target.innerHTML += decoder.decode(value).replace(/\n/g, '<br>')
    }
    }
</script>
