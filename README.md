# Watson-Degradation-Tester

Constantly updating a chatbot causes issues for older content. As you add more information in the harder it generally becomes for a bot to distinguish between intents. 

This can be avoided by reviewing Degradation of confidence before each release. 

The setup is simple 

You have a list of test questions and their expected intent and their previous score in one file and a log of the results in another. I've used CSVs to keep it simple and avoid any other setup.


A file for everything needed is included. 

Python Notebook
Python File
Example Log File
Example Question Set
Example Watson Skill
Example Confusion Matrix
Requirements.txt

This implementation requires you to add one node to your Workspace in order to work. The reason for this is if you access the system provided intent and entities anything below 20% will not return a result. By logging the information inside of Watson as a context and then removing it you get a better view of where things are going wrong rather than returning no results at all. 

