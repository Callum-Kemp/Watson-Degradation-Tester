# Watson Assistant Degradation Tester


### What this is for

This is a tool to keep track of the impact of adding new content to a Chatbot as new intents can create confusion. Using the files and tool provided you can monitor and compare each release to see how your question set is affected by the changes. The purpose of the tool is to allow you to test questions in bulk to make sure that what you predict will happen, happens rather than relying on users receiving the wrong answer and then being fixed from live logs improving customer experience and providing control to future releases. 

You only need one set of data to run this which is a list of ‘control questions’ or prime questions usually at least one per intent. You create a list of these questions and assign them the label of their intent and then measure if your bot has returned the correct intent for that label and what the confidence score is. 

### File setup

There are two CSV files that are read and written to enable this to work. One is a list of questions and previous scores per question and the other is a log of headline results to compare when and where something has gone wrong. These files will be updated in both the Python and Notebook files. The Python file will take the input and send it to your Watson Assistant. 

This uses the IBM Assistant V2 API https://cloud.ibm.com/apidocs/assistant/assistant-v2?code=python 

##### You will need to make sure that you connect your skill to an Assistant to be able to run this.

![Image of File setup](https://i.ibb.co/G5F09WN/Screenshot-2020-09-18-at-17-32-12.png)

### Best practice for your dataset of questions.

- Do not use word for word matches into your intents these will just always return a score of 1 which won’t help .
- Use questions that are similar to ones users are likely to ask. 
- Provide at least one example per intent if you don't you may experience issues with the confusion matrix.

You will only need to make changes to column A and B add your example question that you'd like to test and the intent that question should trigger in column b.

![Image of Question examples](https://i.ibb.co/nb9J1Ky/Screenshot-2020-09-21-at-00-28-16.png)


### Files included

* Python Notebook
* Python File
* Example Log File
* Example Question Set
* Example Watson Skill
* Example Confusion Matrix


### Customisation to Watson Skill required

This implementation requires you to add one node to your Workspace in order to work. The reason for this is if you access the system provided intent and entities anything below 20% will not return a result. By logging the information inside of Watson as a context and then accessing it as part of the payload it will allow you to see which intent something low confidence is leaning towards. If you’d like to use the system only confidence please see the cusotmisation instructions.

![Image of Watson Custom](https://i.ibb.co/n3XYzDw/Screenshot-2020-09-20-at-23-27-53.png)

```python
input={
        'message_type': 'text',
        'text': x,
     'options': {
            'return_context': True
        }
    },
    context={
        'skills': {
            'main skill': {
                'user_defined': {
                    'ConfidenceTester': 'Yes'
                }
            }
        }
    }
    }
```


### Confusion Matrix

A confusion matrix is a great way to check labelled data predictions vs their actual results when there's only a handful of predictions. The results are normalised so if you have 30 examples of intent X and only 2 Examples of intent Y you will get a consitent set of results.  

The below shows a solid red running through the matrix showing the prediction matches the output. 

 
![Image of Confusion Matrix](https://i.ibb.co/pK9QPcc/Confusion-Matrix.png)



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


