from quotes import *
import random

# ------------------------------------------ APP ENTRY POINT ------------------------------------------

def lambda_handler(event, context):
    if event['session']['new']:
        on_start()
    if event['request']['type'] == "LaunchRequest":
        return on_launch(event)
    elif event['request']['type'] == "IntentRequest":
        return on_intent(event)
    elif event['request']['type'] == "SessionEndedRequest":
        return on_end()
        
        
# ------------------------------------------ RESPONSE HANDLERS ------------------------------------------

def on_start():
    print("Session Started.")

def on_launch(event):
    return welcome_response()
    
def on_intent(event):
    
    intent = event['request']['intent']
    session = event['session']
    intent_name = event['request']['intent']['name']

    if intent_name == "TheLoopyCoderIntent":
        return get_quote_response(intent, session)      
    elif intent_name in ["AMAZON.NoIntent", "AMAZON.StopIntent", "AMAZON.CancelIntent"]:
        return stop_the_skill(event)
    elif intent_name == "AMAZON.HelpIntent":
        return assistance(event)
    elif intent_name == "AMAZON.FallbackIntent":
        return fallback_call(event)
    else:
        raise ValueError("Invalid intent")
        

def on_end():
    print("Session Ended.")
        
        
# ------------------------------------------ INTENT HANDLERS -------------------------------------------
    
def welcome_response():
    
    sessionAttributes = {}
    cardTitle = " Welcome "
    speechOutput =  "Hey!!! This is The Loopy Coder, your coder buddy! " \
                    "If you would like to hear some cool programming quotes, you could say for example: Give me a quote!"
    repromptText =  "Do you want to hear some cool programming quotes?" \
                    "Say, give me a quote."
    shouldEndSession = False
    
    return buildResponse(sessionAttributes, buildSpeechletResponse(cardTitle, speechOutput, repromptText, shouldEndSession))

def get_quote_response(intent, session):
    
    coding_quote = random.choice(quotes_list)
        
    cardTitle = "Quote"
    sessionAttributes = {}
    shouldEndSession = True
    todaysQuote = "Today's quote's from " + coding_quote[1] + ". \"" + coding_quote[0] + "\""
    speechOutput = todaysQuote
    repromptText = "Do you want to hear some cool programming quotes?" \
                    "Say, give me a quote."
                        
    return buildResponse(sessionAttributes, buildSpeechletResponse(cardTitle, speechOutput, repromptText, shouldEndSession))

def stop_the_skill(event):
    cardTitle = "Session Ended"
    speechOutput =  "Thank you for hearing The Loopy Coder's quotes. "\
                    "Come back tomorrow for a new interesting coding quote. Have a nice day! "
    repromptText = "Bye bye."
    shouldEndSession = True
    return buildResponse({}, buildSpeechletResponse(cardTitle, speechOutput, repromptText, shouldEndSession)) 
    
def assistance(event):
    sessionAttributes = {}
    cardTitle = " Help "
    speechOutput =  "Hey!!! If you would like to hear some cool programming quotes, you could say, Give me a quote!"
    repromptText =  "Do you want to hear some cool programming quotes?" \
                    "Say, give me a quote."
    shouldEndSession = False
    return buildResponse({}, buildSpeechletResponse(cardTitle, speechOutput, repromptText, shouldEndSession))
    
def fallback_call(event):
    sessionAttributes = {}
    cardTitle = " Wrong question "
    speechOutput =  "Sorry! I can't help you with that, try rephrasing the question or ask for help by saying HELP."
    repromptText =  "Do you want to hear some cool programming quotes?" \
                    "Say, give me a quote."
    shouldEndSession = False
    return buildResponse({}, buildSpeechletResponse(cardTitle, speechOutput, repromptText, shouldEndSession))


# ------------------------------------------ SPEECH RESPONSE HANDLERS ------------------------------------------

def buildSpeechletResponse(title, output, repromptTxt, endSession):
    return {
        'outputSpeech': {
            'type': 'SSML',
            'ssml': "<speak>" +output+"</speak>"
            },
            
        'card': {
            'type': 'Simple',
            'title': title,
            'content': output
            },
            
        'reprompt': {
            'outputSpeech': {
                'type': 'SSML',
            'ssml': "<speak>" +repromptTxt+"</speak>"
                }
            },
        'shouldEndSession': endSession
    }


def buildResponse(sessionAttr , speechlet):
    return {
        'version': '1.0',
        'sessionAttributes': sessionAttr,
        'response': speechlet
    }
