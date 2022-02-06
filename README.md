# wa-minigames
A module that helps you to create minigames with <a href="https://github.com/pedroslopez/whatsapp-web.js" target="_blank">whatsapp-web.js</a>.
## Installation

```console
npm install whatsapp-web.js
npm install wa-minigames
```
## Example Usage
```js
const { Client } = require('whatsapp-web.js');
const { MiniGames, MiniGame } = require('wa-minigames');
class MyGame extends MiniGame {
    constructor(message, client){
        super();
        this.client = client;
        this.chatId = message._getChatId();
        this.answer = Math.floor(Math.random() * 100).toString();
        this.client.sendMessage(this.chatId, "Game Started! Guess the number!");
    }
    async procMessage(message){
        if (message.body===this.answer){
            await this.client.sendMessage(this.chatId, 'You are right!');
            this.gameOver();
        }else if (!message.fromMe){
            await this.client.sendMessage(this.chatId, 'You are wrong.');
        }
    }
    gameOver(){
        super.gameOver();
    }
}

const client = new Client();
const minigames = new MiniGames();
client.on('qr', (qr) => {
    console.log('QR RECEIVED', qr);
});

client.on('ready', () => {
    console.log('Client is ready!');
});

client.on('message_create', async (msg) => {
    if (msg.body === '!start') {
        await minigames.addGameChat(msg._getChatId(), new MyGame(msg, client));
    }
    minigames.forwardMsg(msg);
});

client.initialize();

```
