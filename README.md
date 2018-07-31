*********** READ THROUGH COMPLETELY BEFORE STARTING ***********

STEP 1) In terminal, run the command:

pip install jsonpickle

STEP 2) In views.py:
from djangounchained_flash import ErrorManager, getFromSession

******INITIALIZE********
def index(request):
    if 'flash' not in request.session:
	request.session['flash']=ErrorManager().addToSession()
    e=getFromSession(request.session['flash'])
    #SAMPLE APPLICATION BELOW
    name_errors=e.getMessages('name')
    description_errors=e.getMessages('description')
    #END OF SAMPLE
    request.session['flash']=e.addToSession()

    context={}
    context['name_errors']=name_errors
    context['description_errors']=description_errors
    
******ADDING TO FLASH MESSAGES*******
e=getFromSession(request.session['flash'])
e.addMessage('Name cannot be empty', 'name')
e.addMessage('Email address invalid', 'email')
request.session['flash']=e.addToSession()

******INCLUDED FUNCTIONS******

ErrorManager.addMessage(message, tag=optional)
    --pass in a string and a tag parameter for easy access
ErrorManager.getMessages(tag=optional)
    --get all messages, or only the messages with the passed in tag
    --Will return a list of strings
    --Messages will be shown once, then removed from ErrorManager

******IMPORTANT NOTES********

Anytime you want to modify or save the ErrorManager, between session and normal code, you must convert between getFromSession() and addToSession()

(I tried, Django doesn't seem to allow for an easier way)

*Adding to request.session
    request.session['flash']=<instance of ErrorManager>.addToSession()

*Getting from request.session
    e=getFromSession(request.session['flash'])
