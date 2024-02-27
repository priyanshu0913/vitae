# VITae - The College Companion
### Project by Priyanshu Saraswat
Welcome to the VITae project, a College Companion. This README provides an overview of the project, including the tech stack, setup instructions, file descriptions, and key functionalities.

# Tech Stack
- **Rasa Framework**: For building the conversational AI chatbot.
- **Python**: Used for scripting and backend development.
- **React**: For developing the user interface and frontend components.
- **MySQL**: As the database for storing relevant information.

## Create a Virtual Environment

```bash
python -m venv venv
```

## Run Virtual Environment

```bash
.\venv\Scripts\activate
```

### Populate the data and train model by 

```bash
rasa train
```

### Start the rasa server

```bash
rasa run --cors "*" --enable-api
rasa run actions
```

### Start the ui

```bash
cd vitae-ui
npm start
```
---

# Project Files

- **config.yml**: This file is used to configure the NLU (Natural Language Understanding) and Core components of chatbot. It includes settings such as pipeline configuration, policies for dialogue management, and other training-related parameters.

- **domain.yml**: This file defines the domain of chatbot, including the intents, entities, responses, actions, and templates. It provides a high-level overview of what chatbot can do and helps in organizing and structuring the conversation.

- **nlu.yml**: This file contains the training data for the NLU model. It includes examples of user messages annotated with intents and entities. The NLU model is trained based on this data to understand and classify user input.

- **stories.yml**: This file contains sample dialogues or stories that are used to train the dialogue management model. Each story consists of a sequence of user messages and corresponding bot actions.

- **credentials.yml**: This file is used to store credentials for external services, such as APIs or databases, that your chatbot may interact with during conversations.

- **Basic.js**: This JavaScript file plays a crucial role in handling user interactions and integrating with the Rasa server.

- **chatBot.css**: Provides styling for the chatbot user interface.
---

# Fetch Data

- This function is responsible for making a POST request to the Rasa server's webhook endpoint It sends JSON data in the request body containing the sender's name and the user's message.

```javascript
const rasaAPI = async function handleClick(name, msg) {
    await fetch('http://localhost:5005/webhooks/rest/webhook', {
        method: 'POST',
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json',
            'charset':'UTF-8',
        },
        credentials: "same-origin",
        body: JSON.stringify({ "sender": name, "message": msg }),
    })
    .then(response => response.json())
    .then((response) => {
        if(response) {
            const temp = response[0];
            const recipient_id = temp["recipient_id"];
            const recipient_msg = temp["text"];        

            const response_temp = {sender: "bot", recipient_id: recipient_id, msg: recipient_msg};
            setbotTyping(false);
            setChat(chat => [...chat, response_temp]);
        }
    });
}
```


---

- This function is triggered when the user submits a message.It creates a request object with user details and the input message.

```javascript
const handleSubmit = (evt) => {
    evt.preventDefault();
    const name = "shreyas";
    const request_temp = {sender: "user", sender_id: name, msg: inputMessage};
    
    if (inputMessage !== "") {
        setChat(chat => [...chat, request_temp]);
        setbotTyping(true);
        setInputMessage('');
        rasaAPI(name, inputMessage);
    } else {
        window.alert("Please enter a valid message");
    }
}
```

---

VITae is designed to elevate the college experience through its interactive chatbot interface. The integration of Rasa, React, and MySQL ensures a seamless and engaging user experience, offering a personalized and dynamic interaction platform.
Should you have any queries, innovative ideas, or constructive suggestions, your valuable input will play a pivotal role in elevating the impact and user-friendliness of this project. I welcome your expertise and insights, as they contribute significantly to the continuous improvement and refinement of VITae.#
