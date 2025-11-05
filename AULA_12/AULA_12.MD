Inteligência com NLP (Natural Language Process)
Objetivos:
- Entender o que é classificação de intenções (Intent Classification)
- Aplicar NLP + Machine Learning para ensinar o chatbot a “entender” comandos
- Implementar um modelo simples com Naive Bayes usando scikit-learn

1 - O que é uma Intenção?
Quando um usuário fala com o chatbot, ele quer algo específico — isso é a intenção.
Exemplos:
 - “Quero comprar um produto.” → intenção: comprar

 - “Quero cancelar meu pedido.” → intenção: cancelar

 - “Qual o horário de funcionamento?” → intenção: saber_horario

Analogia do garçom: ele não precisa entender toda a frase do cliente, só precisa identificar o que o cliente quer — pedir, cancelar ou perguntar algo.

2 - Fluxo Lógico
- O usuário digita uma mensagem.
- O texto é pré-processado (limpeza, minúsculas, remoção de stopwords).
- O modelo classifica a intenção (classe) da frase.
- O chatbot responde conforme a intenção reconhecida.

3️ - Implementação em Naive Bayes
Etapa 1: Dataset de Treino
# Exemplo de dataset
frases = [
    "Quero comprar um produto",
    "Gostaria de fazer um pedido",
    "Quero cancelar minha compra",
    "Desejo cancelar o pedido",
    "Qual é o horário de funcionamento?",
    "Vocês abrem aos sábados?"
]

intencoes = [
    "comprar",
    "comprar",
    "cancelar",
    "cancelar",
    "saber_horario",
    "saber_horario"
]

Etapa 2: Vetorização do Texto

Convertendo frases em números com TF-IDF:

from sklearn.feature_extraction.text import TfidfVectorizer

vetorizador = TfidfVectorizer()
X = vetorizador.fit_transform(frases)

Etapa 3: Treinando o Modelo
from sklearn.naive_bayes import MultinomialNB

modelo = MultinomialNB()
modelo.fit(X, intencoes)

Etapa 4: Testando o Modelo
nova_frase = "Gostaria de saber o horário da loja"
vetor = vetorizador.transform([nova_frase])
previsao = modelo.predict(vetor)

print("Intenção reconhecida:", previsao[0])


Saída esperada:

Intenção reconhecida: saber_horario


APIs e Comunicação entre Sistemas
Objetivos:
- Entender o conceito de API REST
- Criar endpoints com Flask
- Enviar e receber dados em JSON
- Integrar o chatbot via requisições HTTP


1️ - O que é uma API REST
API (Application Programming Interface) = meio de comunicação entre sistemas
REST = arquitetura baseado em requisições HTTP (GET, POST, PUT, DELETE)

Analogia:Pense em uma API como um garçom que leva o pedido (requisição) do cliente até a cozinha (servidor) e depois traz a comida (resposta).

2️ - Criando uma API com Flask
Instalação
pip install flask

Exemplo de Endpoint
from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route("/chat", methods=["POST"])
def chatbot():
    data = request.get_json()
    mensagem = data.get("mensagem", "").lower()

    if "horário" in mensagem:
        resposta = "Atendemos das 8h às 18h."
    elif "preço" in mensagem:
        resposta = "Os preços variam conforme o produto."
    else:
        resposta = "Desculpe, não entendi."

    return jsonify({"resposta": resposta})

if __name__ == "__main__":
    app.run(port=5000, debug=True)

3️ - Testando a API

Use Postman para testar:

POST http://localhost:5000/chat
Body (JSON):
{
  "mensagem": "Qual o horário de funcionamento?"
}


Resposta esperada:

{"resposta": "Atendemos das 8h às 18h."}



Integração com Plataformas Reais
Objetivos:
- Aprender a conectar o chatbot ao Telegram, Discord ou WhatsApp
- Configurar bot tokens e webhooks
- Entender o fluxo de mensagens simultâneas e múltiplos usuários
- Fazer deploy real do chatbot

1️ - Conceito de Integração

As plataformas (Telegram, WhatsApp) oferecem APIs públicas.

Cada mensagem enviada pelo usuário é encaminhada via webhook para o servidor do chatbot.

O chatbot processa a mensagem e responde via requisição POST.

2️ - Exemplo: Chatbot no Telegram
Passos Iniciais

No Telegram, procure @BotFather

Crie um novo bot → receba um token de API

Guarde o token (ex: 123456:ABCDEF)

Código Python + Flask
from flask import Flask, request
import requests

TOKEN = "SEU_TOKEN_AQUI"
app = Flask(__name__)

@app.route(f"/{TOKEN}", methods=["POST"])
def responder():
    data = request.get_json()
    chat_id = data["message"]["chat"]["id"]
    texto = data["message"]["text"].lower()

    if "oi" in texto:
        resposta = "Olá! Sou o bot da empresa."
    elif "horário" in texto:
        resposta = "Atendemos das 8h às 18h."
    else:
        resposta = "Desculpe, não entendi."

    enviar_mensagem(chat_id, resposta)
    return "ok"

def enviar_mensagem(chat_id, texto):
    url = f"https://api.telegram.org/bot{TOKEN}/sendMessage"
    requests.post(url, json={"chat_id": chat_id, "text": texto})

if __name__ == "__main__":
    app.run(port=5000)

Configurando o Webhook
Execute no terminal:

curl -F "url=https://seuservidor.com/SEU_TOKEN_AQUI" \
https://api.telegram.org/botSEU_TOKEN_AQUI/setWebhook

