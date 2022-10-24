# Integração Slack e Python 

[![Python 3.6.8](https://img.shields.io/badge/python-3.6-blue.svg)](https://www.python.org/downloads/release/python-360/)

![N|Solid](https://cdn.swapps.com/uploads/2019/07/como-integrar-tu-aplicaci%C3%B3n-python-a-slack-usando-bots-landscape.jpg)
<br>
<br>
Este guia é feito para orientar uma integração básica entre o aplicativo Slack e o Python , usando a biblioteca Slack-Bolt para Python.
💡 Em nosso exemplo, vamos usar o modo soquete, nossa opção recomendada para aqueles que estão apenas começando e construindo algo para sua equipe. São necessário passos diferentes , se você for usar HTTP como protocolo de comunicação do seu aplicativo.


## Crie um aplicativo

![N|Solid](https://th.bing.com/th/id/R.0e3c0c0304660d038b58c8b9267d535e?rik=KmjWoMxrMiauGw&pid=ImgRaw&r=0)

<br>
<br>

A primeira coisa é: criar um app no slack
Entre em https://api.slack.com/apps, e clique no botão Create New App
Depois de preencher um nome de aplicativo (você pode alterá-lo mais tarde) e escolher um espaço de trabalho para instalá-lo, aperte o botão e você pousará na página de Informações Básicas do seu aplicativo.
Esta página contém uma visão geral do seu aplicativo, além de credenciais importantes que você deseja referenciar mais tarde.

<br>
<br>

![N|Solid](https://camo.githubusercontent.com/ffb849afd39776f8d7e3d27a3bcc5ab02591b87b52fd1ead7a14ffde898f4ce0/68747470733a2f2f692e696d6775722e636f6d2f514465754e4a4d2e706e67)
    
<br>
<br>

# **Tokens e instalação de aplicativos**

Os aplicativos slack usam o OAuth para gerenciar o acesso às API’s do Slack. Quando um aplicativo é instalado, você receberá um token que o aplicativo pode usar para chamar métodos de API.
Existem três principais tipos de tokens disponíveis para um aplicativo Do Slack: tokens de usuário (), bot () e nível de aplicativo ().
Vamos usar tokens de bot e nível de aplicativo para este guia.

1. Navegue até o OAuth & Permissions na barra lateral esquerda e role até a seção Bot Token Scopes. Clique em Add on OAuth Scope.
2. Por enquanto, vamos adicionar apenas um escopo:** chat:write**. Isso concede ao seu aplicativo a permissão para postar mensagens em canais dos seus membros.
3. Role até o topo da página OAuth & Permissions e clique em Install to workspace. Você será conduzido através da Interface do Usuário OAuth do Slack, onde você deve permitir que seu aplicativo seja instalado no seu espaço de trabalho de desenvolvimento.
4. Uma vez autorizado a instalação, será apresentado o  Bot User OAuth Access Token. Você deve copiá-lo

<br>
<br>

![N|Solid](https://camo.githubusercontent.com/d4addbd5f0ff7bf00034c4823548042b0991b28d6c64b1b9d3aa035765efc70f/68747470733a2f2f692e696d6775722e636f6d2f6b517a746e68442e706e67) 

<br>
<br>

5. Em seguida, vá até Basic Information e role para baixo na seção App-Level Tokens e clique em Generate Token and Scopes para gerar um token em nível de aplicativo. Adicione o escopo a este token e salve o token gerado, usaremos esses dois tokens em apenas um momento: `connections:write`
6.Navegue até o menu Socket mode e habilite-o.

<br>
<br>

![N|Solid](https://camo.githubusercontent.com/504f03217939eba5fae2166a0bd012064b408b8eb00a08311f3be34777b3cca7/68747470733a2f2f692e696d6775722e636f6d2f4f6155645a544d2e706e67) 

<br>
<br>

# Configuração de Eventos

Para ouvir os eventos que acontecem em um espaço de trabalho do Slack , nós usaremos a API de eventos do slack.

O Modo Soquete permite que os aplicativos usem a API de Eventos e componentes interativos sem expor um ponto final HTTP público. Isso pode ser útil durante o desenvolvimento, ou se você estiver recebendo solicitações por trás de um firewall. HTTP é mais útil para aplicativos que estão sendo implantados em ambientes de hospedagem ou aplicativos destinados à distribuição através do Slack App Directory.

É hora de dizer ao Slack que eventos gostaríamos de ouvir. Quando um evento ocorre, o Slack enviará ao seu aplicativo algumas informações sobre o evento, como o usuário que o acionou e o canal em que ocorreu. Seu aplicativo processará os detalhes e poderá responder de acordo.

Role para baixo para assinar eventos de bot. Existem quatro eventos relacionados a mensagens:

- `message.channels` ouve mensagens em canais públicos que seu aplicativo é adicionado
- `message.groups` ouve mensagens em 🔒 canais privados que seu aplicativo é adicionado a
- `message.im` ouve mensagens nos DMs do seu aplicativo com usuários
- `message.mpim` ouve mensagens em DMs multi-pessoas que seu aplicativo é adicionado

Se você quiser que seu bot ouça mensagens de todos os lugares para os quais seja adicionado, escolha todos os quatro eventos de mensagens. Depois de selecionar os eventos que deseja que seu bot ouça, clique no botão verde Salvar alterações.

![N|Solid](https://camo.githubusercontent.com/f92ce5d82ec07bb7056cc349a542b5393eeb5cacaed8efed02714ea624eb1018/68747470733a2f2f692e696d6775722e636f6d2f547239613230332e706e67) 

# Configuração para seu projeto

## Setup


```bash
# Python 3.6+ required

#no seu terminal de preferencia digital os seguinte comandos  para a instalação das bibliotecas requeridas

- pip install slackclient
- pip install slack_bolt

```
  Agora vamos criar nossos primeiros eventos em no código vamos seguir os seguintes passos
  
### **1. Criamos um arquivo en nosso editor de codigos preferido e criamos o arquivo com a extensão .py example app.py**
<br>

![N|Solid](https://user-images.githubusercontent.com/91167097/164299206-01054ac3-e5ed-4f7a-98db-f7c13812b961.png) 

<br>
<br>

### 2. Copiar os 2 tokens: " **bot e app** " en nosso exemplo  ilustrado
##### token a copiar seguir a ilustração

![N|Solid](https://user-images.githubusercontent.com/91167097/164295910-2aa6f7c9-aed9-4be9-b9d3-fb61d8ad631d.png) 
![N|Solid](https://user-images.githubusercontent.com/91167097/164296450-d9709391-556b-4b24-a617-6306a12d7f78.png) 


**2. Copiar os 2 tokens: " **bot e app** " en nosso exemplo  ilustrado**


### Criando um aplicativo e Executando en modo de Socket ( SocketModeHandler)

```sh
from slack_bolt import App
from slack_bolt.adapter.socket_mode import SocketModeHandler

# Inicia o objeto app passando o token do bot
app = App(token="SLACK_BOT_TOKEN")

# Ouve todas as mensagens recebidas no canal

@app.message("oi")
def message_slack(message, say):
    resposta = "que poso ajudar  "  # resposta do bot para o usuario
    say(f"{resposta}") #manda a resposta para o slack


## inicia o app passando o token do app
if __name__ == "__main__":
 SocketModeHandler(app, "SLACK_APP_TOKEN")
```

Se deu certo, nosso bot já está devidamente sincronizado 

# Enviar arquivos

<br>

![N|Solid](https://assets-global.website-files.com/610d78d90f895fbe6aef8810/6207c7a5a6e1727158dd49d2_608fb6511982b87e6f4b46ed_Untitled-design-20-1.jpeg) 

Para poder enviar arquivos, você deve ativar o seguinte scope no OAuth & Permissions **(não esqueça de reinstalar o bot depois de fazer as alterações do scope) **


 
 ![N|Solid](https://user-images.githubusercontent.com/91167097/164313418-439e7024-aa97-494d-9797-504dfb723ca6.png) 

## Criando um aplicativo e Executando um aplicativo
```sh
import os
from slack_bolt import App
from slack_bolt.adapter.socket_mode import SocketModeHandler

#rota do arquivo a ser enviado 
filename='./wally.jpg'

# cria o objeto app com o token do bot
app = App(token=SLACK_BOT_TOKEN)

# Ouve todas as mensagens recebidas no canal
@app.message("hello")
def message_slack(client,message, say):
    resposta='hello human and I send you the file' # resposta do bot para o usuario
    say(f"{resposta}") #manda a resposta para o slack
    channel_id=message["channel"]
    result=client.files_upload(channels=channel_id,initial_comment='successful download thanks for waiting',file=filename)

## inicia o app passando o token do app
if __name__ == "__main__":
    SocketModeHandler(app, SLACK_APP_TOKEN).start()
```

 ![N|Solid](https://user-images.githubusercontent.com/91167097/164315603-a6f7116d-f5fd-4ec8-a285-3fa0ac0e820f.png) 
 
 <br>
 
Se deu certo, nosso bot já está devidamente sincronizado e pronto para enviar arquivos

 ![N|Solid](https://th.bing.com/th/id/OIP.FWar-rSERkpkmpFSQqyQ0AAAAA?pid=ImgDet&rs=1) 
 
<br>
<br>

# Integração con ChatterBot

![N|Solid](https://chatterbot.readthedocs.io/en/stable/_images/banner.png) 

# Acerca de ChatterBot
ChatterBot é uma biblioteca Python que facilita a geração de respostas automatizadas à entrada de um usuário. O ChatterBot usa uma seleção de algoritmos de aprendizado de máquina para produzir diferentes tipos de respostas. Isso facilita para os desenvolvedores criar bots de bate-papo e automatizar conversas com usuários. Para obter mais detalhes sobre as ideias e conceitos por trás do ChatterBot, consulte o diagrama de fluxo do processo .

Um exemplo de entrada típica seria algo assim:

```
user: Good morning! How are you doing?
bot:  I am doing very well, thank you for asking.
user: You're welcome.
bot:  Do you like hats?
```

Como o ChatterBot funciona
ChatterBot é uma biblioteca Python projetada para facilitar a criação de software que pode se envolver em conversas.

Uma instância não treinada do ChatterBot começa sem nenhum conhecimento de como se comunicar. Cada vez que um usuário insere uma instrução , a biblioteca salva o texto inserido e o texto ao qual a instrução foi respondida. À medida que o ChatterBot recebe mais entrada, o número de respostas que ele pode responder e a precisão de cada resposta em relação à instrução de entrada aumentam.

O programa seleciona a resposta de correspondência mais próxima pesquisando a declaração conhecida de correspondência mais próxima que corresponde à entrada e, em seguida, escolhe uma resposta da seleção de respostas conhecidas para essa declaração.


Diagrama de fluxo do processo ChatterBot

![N|Solid](https://chatterbot.readthedocs.io/en/stable/_images/chatterbot-process-flow.svg) 

# Construindo um Chatbot usando Chatterbot , Docker , Paython e se conectar com slack

Comencemos instalando la biblioteca de chatterbot lo qual usaremos uma imagem com docker 
passos a seguir 

### 1. Criar un arquivo **Dockerfile**

```sh
# Defina a imagem base para  python,  importante o chatterbot  só será capaz de executar com a versão 3.6.8
FROM python:3.6.8-slim 

WORKDIR /app

# Instalando as bibliotecas requeridas
COPY requirements.txt requirements.txt
RUN pip install --upgrade pip==21.3.1
RUN pip3 install -r requirements.txt
RUN pip3 install chatterbot==1.0.4

# token necesarios 
COPY . .
ENV SLACK_APP_TOKEN=
ENV SLACK_BOT_TOKEN=
CMD [ "python3"," app.py"]
#app.py  refere-se ao arquivo que contém seu código de chatterbot

```
<br>
    
    
### 2. Criar un arquivo **requirements.txt**
    
```sh
#todas as bibliotecas que precisa o chattebot a qual Dokerfile vai a fazer o requerimentos para instalar na imagen
slack
slack_bolt
slackclient
spacy
pytz
python-dateutil
six
mathparse
thinc
numpy
dataclasses
pydantic
typing-extensions
contextvars
srsly
wasabi
blis
cymem
mmh3
preshed
```
<br>

### 3. Criando nosso código para ou chatterbot

```sh
import os
import json
from slack_bolt import App
from slack_bolt.adapter.socket_mode import SocketModeHandler
from chatterbot import ChatBot

chatbot = ChatBot("Ron Obvious")
from chatterbot.trainers import ListTrainer

# cria o objeto app com o token do bot
app = App(token=os.environ.get("SLACK_BOT_TOKEN"))

chat = [
'Hi',
'Hello',
'I need your assistance regarding my order',
'Please, Provide me with your order id',
'I have a complaint.',
'Please elaborate, your concern',
'How long it will take to receive an order ?',
'An order takes 3-5 Business days to get delivered.',
'Okay Thanks',
'No Problem! Have a Good Day!'
]

neobot = ListTrainer(chatbot)  # carregando lista de valores
neobot.train(chat)  # treinando o robô


# Ouve todas as mensagens recebidas no canal

@app.message("")
def message_slack(message, say):
    resposta = chatbot.get_response(
        message['text']
    )  # busca a mensagem recebida na base de aprendizado e traz a resposta
    say(f"{resposta}")  #manda a resposta para o slack


# Inicia o app
if __name__ == "__main__":
    SocketModeHandler(app, os.environ["SLACK_APP_TOKEN"]).start()

```
<br>

Agora vamos colocar o nosso bot para trabalhar ,como estamos trabalhando com docker desde que terminamos todo o nosso código temos que criar nossa imagem usando o seguinte comando e **importante para destacar que temos que ter docker instalado em nossa máquina se você não tiver, você tem que  instalá-lo na sua maquina **

```bash
docker build . -t teste
```

![N|Solid](https://user-images.githubusercontent.com/91167097/164775517-ab17bb3c-4f85-4420-809b-b9d2cf4befea.png) 


Agora para executar nosso contêiner
```bash
docker run --rm  teste
```
![|Solid](https://user-images.githubusercontent.com/91167097/164777271-70e8dcbd-55d2-4948-b57e-bbd6f3a71bb4.png)

<br>
<br>

# Conclusión

¡Parabéns, você chegou ao fiml

Aqui foi baseado em aprender como fazer um chatbot em Python usando a biblioteca ChatterBot e slack e ser capaz de conectar a API do Slack. Construir um chatbot com o ChatterBot não foi apenas simples, mas também os resultados foram precisos. Com inteligência artificial e aprendizado de máquina, em avanço, tudo e qualquer coisa é possível de alcançar, seja criando bots com habilidades de conversação como humanos ou qualquer outra coisa.

Agora  você pode Brincan um pouco e criar o seu bot para o seu canal de trabalho para simplificar o trabalho de sua equipe


# Documentaçao
- https://slack.dev/bolt-python
- https://api.slack.com/apps,
- https://chatterbot.readthedocs.io/en/stable/index.html
- https://docs.microsoft.com/pt-br/dotnet/architecture/microservices/container-docker-introduction/docker-defined


# Contribuidores 
- Fernanda Paulino Leite- Arquiteta de Dados
- Caio Sena Souto  Analista de Tecnologia
- Marcos de Oliveira Cucoro - Analista de Tecnologia
- Esteban Jose Gonzalez Gome - Analista de Tecnologia
- Thiago Pereira de Souza - Engenharia - DevOps





------------




