---
title: "What is an AI Agent?"
image: "/what-is-an-ai-agent/aiagents.jpg"
summary: "AI agents are one of the most hyped words in the AI space. The word is thrown left and right like candy and it definitely helps you to sound smart with no consensus on a definition. I'm a simple man, so I need a simple definitions. So, after reading a bunch of stuff and building my own agents, I think this definition... ...makes me happiest."
date: 2024-12-12T22:39:02+01:00
---


What is an AI Agent and how should we define it?

![AI Agents](/what-is-an-ai-agent/aiagents.jpg)

AI agents are one of the most hyped words in the AI space. The word is thrown left and right like candy and it definitely helps you to sound smart with no consensus on a definition. I'm a simple man, so I need a simple definitions. So, after reading a bunch of stuff and building my own agents, I think this definition... ...makes me happiest. 

> "Automations but Better"


AI agents are nothing but automations on drugs 💊. A classic automation flow is when something is done on a recurring basis or when an event triggers some other event. For example, if I define an email rule to forward all my emails to another destination, that's a simple automation. To make it a bit more advanced, if you add a condition to check the sender, and if it's sender A, forward it, and if it's from the sender B, just archive it or send it to spam.This is slightly more detailed automation.

Now imagine that you don't want to define all the conditions for your automation or it's just impossible because the conditions are not clear. And you want the system to decide what's the next action to take. In this case you can use an AI model to reason and make a decision on your behalf, and based on that, choose and execute the next action. That's an example of an "agentic behavior". An example of that would be to check the content of my incoming emails and if it's a marketing email archive it but if it's personal emails or from people in my contact list then mark them as important and add them in my todo list. 

## Multi-agent systems
Now, one of the key mechanism of large language models is that the better they can focus, the better they do a task. And that's done through an attention mechanism. You don't want to ask a model to do multiple different tasks in one attempt. Because of the attention mechanism, the output will suffer. What you can do instead is that once an agent is finished with a certain task, you pass it on to a different agent for the next round of action. This is a multi-agent system.

For example, you may want to search Google for the new AI trends, summarize them and write an article about them. This operation involves multiple steps. The first one would be to search Google, get an output of X results, then summerize each of those articles into key points and finally compile a coherent article from all of them. 

When building such a system, you don't want to do all these tasks in the same session. Large language models, are subject to recency bias and when the context window is polluted with too many unrelated content, the quality of the output decreases significantly. The intuitive way to think about it is multitasking in humans. As the number of tasks at hand increases, the attention span for each task decreases.

 To build our article generator, you'd be better of using separate agents specialising on their own task. One would be in charge of searching the content, one in charge of summarizing it and the last in charge of writing. Agent 1 first searchse and then pass that search result to the summarizer. The summarizing agent will summarize the result and passes to the writer. And finally the write, takes all that and writes the article.

 Multi-agent systems have shown to be quite promising and of much higher quality compared to single agent systems. There are a lot of frameworks at the moment that can orchestrate a multi-agent system. One example is [CrewAI](crewai.com). OpenAI resets a new tool called [Swarm](https://github.com/openai/swarm), an experimental tool. 

In the coming days, I will be writing more about practical implementation of agents using various frameworks. Stay tuned.




