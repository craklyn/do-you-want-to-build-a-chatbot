# do-you-want-to-build-a-chatbot

# Install rasa

`pip install rasa-x --extra-index-url https://pypi.rasa.com/simple`

# Create new project

```
mkdir hello-chatbot-world
cd hello-chatbot-world
rasa init --no-prompt
```

Once you've answered a couple interactive questions, a baseline model will be trained before your eyes.  

Note that you may see some Tensorflow warnings.  These can be safely ignored.


# Babby's first conversation

Enter into a conversation with the chatbot by entering `rasa shell`.

The bot is pretty naive.  Here are two example conversation:

```
Your input ->  Hello chatbot world!                                                                                                                                           
Hey! How are you?

Your input ->  Great.  What is your purpose?                                                                                                                                  Great carry on!

Your input ->  What is a camera?
Your input ->  Do you know what your purpose is?
Your input ->  How are you?
Bye
```

```
Your input ->  Hi botty 
Hey! How are you?

Your input ->  I'm sad
Here is something to cheer you up:
Image: https://i.imgur.com/nGF1K8f.jpg
Did that help you?

Your input ->  Yes
Great carry on!

Your input ->  Thanks
Bye

Your input ->  Why are you saying bye?
Bye

Your input ->  Okay..
Bye
```

You can end your chat session by typing `control`+`c`.

The bot is naive now, but we can improve it.

![Arnold Schwarzenegger in an early role before he realized his true calling was being a kindergarten teacher.](arnold.png)

# Key configurables

Intents, actions, and templates are configured in domain.yml.  You can see the contents of this file by running `cat domain.yml`, which will show the following.

```
intents:
  - greet
  - goodbye
  - affirm
  - deny
  - mood_great
  - mood_unhappy

actions:
- utter_greet
- utter_cheer_up
- utter_did_that_help
- utter_happy
- utter_goodbye

templates:
  utter_greet:
  - text: "Hey! How are you?"

  utter_cheer_up:
  - text: "Here is something to cheer you up:"
    image: "https://i.imgur.com/nGF1K8f.jpg"

  utter_did_that_help:
  - text: "Did that help you?"

  utter_happy:
  - text: "Great carry on!"

  utter_goodbye:
  - text: "Bye"
  ```

  Intents represent what the user (human) is expressing to the bot.  Actions represent actions that the bot wants to take in response.  (To repeat: the bot receives text that it categorizes into intents and emits actions.)

  Templates represent ways that the bot expresses its actions.  Just like us humans, a bot can express the same action in different ways.  However, the bot currently only expresses each action in one way.

### Updating bot and interacitvely testing

Let's try making a minimal change and confirming that it changes the behavior of the bot.  Edit domain.yml's utter_greet to have a new message.  Something like this:

```
  utter_greet:
  - text: "Hey bud, how have you been??"
```

Retrain by running `rasa train`.  If you enter the chat with `rasa shell` and say hi, you should see the bot issue the new greeting.  SUCCESS!

You can also create a variety of ways for the bot to express a greeting.  The bot appears to select the expressions at random.

```
  utter_greet:
  - text: "Hey! How are you?1"
  - text: "Hey! What up!"
  - text: "Hey! How the heck have yoou been?"
  - text: "Hey!!!  What's good?"
  ```

  In general, the cycle of making a change, running `rasa train`, and then chatting with the bot in `rasa shell` is an effective building and testing cycle.  (Of course, it won't scale to a fully engineered solution, regression awareness, etc.)