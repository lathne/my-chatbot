# React App Azure Open AI integration Training


## Using the Javascript SDK with ReactJS

Introduction
This guide walks you through the process of creating a chatbot using ReactJS for the front end and Azure OpenAI for generating responses.

Prerequisites
Basic knowledge of JavaScript and ReactJS
Node.js installed
An Azure account
Access to Azure OpenAI services


Step 1: Setting Up Your React Application
Create a new directory and navigate to it

Create a New React App in a command line: console Run 
npx create-react-app my-chatbot

Navigate into your project with cd my-chatbot

You’ll need axios for API requests: console Run 
npm install axios

Structure Your Project:
Create a new file Chatbot.js in the src folder for your chatbot’s UI and logic.

Step 2: Setting Up Azure OpenAI
Create Azure OpenAI Resource:

Log in to your Azure Portal.
Create a new resource for Azure OpenAI service.
Follow the setup instructions and note down the endpoint and key provided.

Resource Group: LabOpenAITraining
Resource: Azure Open AI - lab-open-ai-resource
Subscription ID: c8f1b97e-0310-4d8b-9a22-9055c73973cf

Endpoint: https://lab-open-ai-resource.openai.azure.com/
Location: eastus
Key 1: f243729dd1314ad4945450bcb880861d
Key 2: 02933269768d4065a95fd079ca02142b



Create a deployment using gpt-35-turbo. Note the name for future use
Configure Environment Variables:

In your React project, create a .env file in the root level of the project.
Add your Azure OpenAI endpoint and key:
zoom codecopy code
REACT_APP_AZURE_OPENAI_ENDPOINT=<your_endpoint>
REACT_APP_AZURE_OPENAI_KEY=<your_key>
Step 3: Integrating Azure OpenAI with React
Setting Up API Calls:

In src, create a new file called api.js.
Configure axios to use Azure OpenAI’s endpoint and authorization key.
zoom codecopy code
import axios from 'axios';

const deploymentName = '<deploymentname>'; // Replace with your deployment name
const version = '2023-05-15'; // Replace with the appropriate API version

const api = axios.create({
    baseURL: `${process.env.REACT_APP_AZURE_OPENAI_ENDPOINT}openai/deployments/${deploymentName}/`,
    headers: {
        'api-key': `${process.env.REACT_APP_AZURE_OPENAI_KEY}`,
        'Content-Type': 'application/json',
    },
});


export const fetchResponse = (prompt) => {
    return api.post(`completions?api-version=${version}`, {
        prompt: prompt,
        max_tokens: 150,
    });
};
Building the Chat Interface in Chatbot.js: ```javascript import React, { useState } from ‘react’; import { fetchResponse } from ‘./api’;

const Chatbot = () => { const [messages, setMessages] = useState([]); const [userInput, setUserInput] = useState(’’);

zoom codecopy code
 const handleInputChange = (event) => {
     setUserInput(event.target.value);
 };

 const handleSubmit = async (event) => {
     event.preventDefault();
     const userMessage = userInput.trim();

     if (!userMessage) return;

     // Update messages with user input
     setMessages(messages => [...messages, { sender: 'user', text: userMessage }]);

     // Clear input field
     setUserInput('');

     // Get response from Azure OpenAI
     const response = await fetchResponse(userMessage);
     const botMessage = response.data.choices[0].text.trim();

     // Update messages with bot response
     setMessages(messages => [...messages, { sender: 'bot', text: botMessage }]);
 };

 return (
     <div className="chatbot">
         <div className="messages">
             {messages.map((message, index) => (
                 <div key={index} className={`message ${message.sender}`}>
                     {message.text}
                 </div>
             ))}
         </div>
         <form onSubmit={handleSubmit}>
             <input type="text" value={userInput} onChange={handleInputChange} />
             <button type="submit">Send</button>
         </form>
     </div>
 );
};

export default Chatbot; ``` ## Steps to Modify App.js

Import the Chatbot Component:

First, ensure that your Chatbot.js component is in the same directory as App.js or adjust the import path accordingly.
In App.js, import the Chatbot component at the top of the file.
zoom codecopy code
import Chatbot from './Chatbot'; // Adjust the path if Chatbot.js is in a different directory
Include the Chatbot Component in App’s Render Method:

In the App component’s render method (or return statement for functional components), include the Chatbot component. This step is crucial for the chatbot to appear in your application. jsx function App() { return ( <div className="App"> <header className="App-header"> {/* Other content */} <Chatbot /> </header> </div> ); }
Remove Placeholder Content:

If App.js contains placeholder content (like the default React logo and welcome message), you might want to remove or modify this to better fit your chatbot application.
Add or Update Styling:

If you have specific styles for your chatbot, ensure they are included in your application’s CSS files. This might involve editing App.css or adding a new stylesheet.
Save and Re-run:

After making these changes, save App.js.
Run npm start
The resulting chatbot

zoom image
jschatbot
jschatbot
Enter a sample prompt and click Send