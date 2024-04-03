---
title: "Extracting Actionable Insights from Customer Calls: A Practical Example"
date: 2024-04-02T07:02:48+02:00
draft: true
---

A lot of businesses do their customer support through phone calls. Usually, customers prefer to reach out for the phone to get their issues solved rather than to get into a ping pong email interaction to solve a problem. It is faster, interactive and more human.

Customer support calls are direct link to the customer's thoughts and needs. Businesses receive direct feedback and critisism into the operations and delivery of the service or product. At the risk of stating the obvious, lots of business decision should be made from the customer feedback.

 Yet, once a problem is solved, these interactions are often overlooked and not analyzed further. Many companies do not even record a copy of the calls. And those that do, mostly make analysis on the metadata rather than the content. Not that they don't want to. But because it was not that available.
 
 Up until not so long ago, it was technically quite challenging to do bulk analsyis of voice data and the content of it. With the introduction of LLMs and modern transcription services, voice data analysis is democraised and no longer needs a team of machine learning engineers.

However, it is now as simple as calling a few API calls to get a transdcription of call and then do a semantic analysis on the content. For example, you can get analytics on the type of calls and get actionable insights.

Here is an example of doing sentiment analysis and offering actionable insights on the following suppport call. The sentiment analysis is fine grained to certain phrases to show the level of details we can go to. And the actions are what to improve. On a sufficiently large dataset, the actions can be further clusterred or classified to and then implemented in the business based on the frequency and importance.

<audio controls>
  <source src="https://github.com/n4cr/ai-cookbook/raw/main/customer_call/customer.mp3" type="audio/mpeg">
Your browser does not support the audio element.
</audio>

Outcome of the analsys:
```json
{
  "overall_sentiment": "neutral",
  "insights": [
    {
      "phrase": "wrong size and my size is not available",
      "sentiment": "negative"
    },
    {
      "phrase": "have not received any email",
      "sentiment": "negative"
    },
    {
      "phrase": "refund beyond the time frame",
      "sentiment": "negative"
    },
    {
      "phrase": "two to four business days",
      "sentiment": "positive"
    },
    {
      "phrase": "database not accepting order number",
      "sentiment": "negative"
    }
  ],
  "actionable_items": [
    {
      "action": "Improve database system to reliably accept and recognize order numbers",
      "importance": "high"
    },
    {
      "action": "Ensure clear communication about return policies, including deadlines",
      "importance": "medium"
    },
    {
      "action": "Streamline the documentation process for returns, including the necessary return tracking information",
      "importance": "medium"
    }
  ]
}
```

The code to produce that. 
```python
import instructor
from openai import OpenAI
from pydantic import BaseModel, Field
from typing import List, Tuple
import json

# Define Pydantic models for the analysis of customer calls
class CallInsight(BaseModel):
    phrase: str = Field(description="A key phrase extracted from the call that holds significant insight.")
    sentiment: str = Field(description="The sentiment expressed regarding the key phrase, e.g., positive, negative, neutral.")

class ActionableItem(BaseModel):
    action: str = Field(description="A suggested action or area for improvement identified from the call.")
    importance: str = Field(description="The level of importance or urgency of the action, e.g., high, medium, low.")

class CallAnalysis(BaseModel):
    overall_sentiment: str = Field(description="The overall sentiment of the call, e.g., positive, negative, neutral.")
    insights: List[CallInsight] = Field(default_factory=list, description="List of key insights extracted from the call.")
    actionable_items: List[ActionableItem] = Field(default_factory=list, description="List of actionable items or suggestions for improvement.")

# Patch the OpenAI client to add response_model support
client = instructor.patch(OpenAI())

def analyze_customer_call(call_text: str) -> CallAnalysis:
    """Analyzes a customer call to extract sentiment, key insights, and actionable items."""
    return client.chat.completions.create(
        model="gpt-4-turbo-preview",
        messages=[
            {
                "role": "user",
                "content": f"Analyze the following customer call for sentiment, key insights, and actionable items: {call_text}"
            }
        ],
        response_model=CallAnalysis
    )

if __name__ == "__main__":
    # Example customer call transcript
    print("Opening audio file for transcription.")
    audio_file = open("customer_call/customer.mp3", "rb")
    print("Transcribing audio file.")
    customer_call = client.audio.transcriptions.create(
        file=audio_file,
        model="whisper-1",
        response_format="text"
    )
    print("Transcription complete. Analyzing the customer call.")
    # Analyze the customer call
    analysis = analyze_customer_call(customer_call)
    
    print("Analysis complete. Printing the analysis in JSON format for readability.")
    # Print the analysis in JSON format for readability
    print(json.dumps(analysis.model_dump(), indent=2))
    print("Process completed.")


```

Which will result in

