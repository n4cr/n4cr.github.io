---
title: "To Stream or Not to Stream"
date: 2024-02-07T12:13:05+01:00
draft: true
---

A big question when designing an AI system is whether to stream the output or not. This is a question that is often overlooked but can have a big impact on the user experience and the performance of the system.

When streaming data is rendered to the user as soon as it is available, rather than waiting for the entire response to be ready. This can be useful in many cases, for example when the response is large and the user needs to see the data as soon as possible. However, streaming can also introduce complexity and overhead, and can be difficult to implement correctly.

For exmaple consider the following example:

<button onclick="fetchData()" data-target="#output" data-t="
    Write a story about a dog.
    ">Write a story about a dog</button>
<div id="output" class="prompt"></div>

<script>
    
    async function fetchData() {
    
        // const t = "tell a story about a dog."
    const button = document.querySelector('button');
    button.disabled = true;
    const t = button.getAttribute('data-t');
    const targetSelector = button.getAttribute('data-target');
    const target = document.querySelector(targetSelector);
    target.innerHTML = '';

    const url = 'http://localhost:5000/events?t=' + t // Replace with your URL
    
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
