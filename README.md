# Watson Assistant Degradation Tester


### What this is for

This is a tool to keep track of the impact of adding new content to a Chatbot as new intents can create confusion. Using the files and tool provided you can monitor and compare each release to see how your question set is affected by the changes. The purpose of the tool is to allow you to test questions in bulk to make sure that what you predict will happen, happens rather than relying on users receiving the wrong answer and then being fixed from live logs improving customer experience and providing control to future releases. 

You only need one set of data to run this which is a list of ‘control questions’ or prime questions usually at least one per intent. You create a list of these questions and assign them the label of their intent and then measure if your bot has returned the correct intent for that label and what the confidence score is. 

### File setup

![Image of File setup](https://i.ibb.co/G5F09WN/Screenshot-2020-09-18-at-17-32-12.png)

### Best practice for your dataset of questions.

Do not use word for word matches into your intents these will just always return a score of 1 which won’t help .
Use questions that are similar to ones users are likely to ask. 
Provide at least one example per intent. 

### Files included

* Python Notebook
* Python File
* Example Log File
* Example Question Set
* Example Watson Skill
* Example Confusion Matrix

This implementation requires you to add one node to your Workspace in order to work. The reason for this is if you access the system provided intent and entities anything below 20% will not return a result. By logging the information inside of Watson as a context and then accessing it as part of the payload it will allow you to see which intent something low confidence is leaning towards. If you’d like to use the system only confidence you can replace 

![Image of File setup](https://i.ibb.co/n3XYzDw/Screenshot-2020-09-20-at-23-27-53.png)


### Customisation

```python
    IntentList.append((response['context']['skills']['main skill']['user_defined']['Intent']['intent']))
    ConfidenceList.append((response['context']['skills']['main skill']['user_defined']['Intent']['confidence']))
```
    
with 

```python
 try:
        ConfidenceList.append(response['output']['intents'][0]['confidence'])
    except:
        ConfidenceList.append(0)
    try:
        IntentList.append(response['output']['intents'][0]['intent'])
    except:
        IntentList.append("ZLowConf")
```

I’ve used the content from the Watson content catalogue for Mortgages for testing and example purposes but this can easily be customised to your own question set by exporting your intents as a CSV and then copying them in and making changes.


The Confusion Matrix allows you to quickly see which intents are incorrectly triggering 
