Chatbot

Programa que simula uma conversa com um ser humano por meio de texto ou voz.
Seu objetivo é responder, orientar ou executar tarefas com base em entradas de linguagem natural.

Tipos principais
Tipo	Características	Exemplos
Baseado em regras	Responde a palavras-chave e fluxos definidos	Chatbots de FAQ
Baseado em IA/NLP	Interpreta intenção e contexto por Machine Learning	Assistentes inteligentes (Alexa, ChatGPT)

Estrutura de funcionamento
1.	Input: o usuário envia uma mensagem.
2.	Processamento (NLP): o bot interpreta o que foi dito (intenção e entidades).
3.	Response(ação): o bot executa uma ação (resposta, busca, cálculo).
4.	Output: o bot devolve uma mensagem ao usuário.

Fluxo lógico:
Usuário: "Quais são os horários de atendimento?"
Bot: "Nosso atendimento é das 8h às 18h de segunda a sexta!"
Fluxo interno:
1.	Identificar a intenção: “consultar horário”.
2.	Buscar a resposta associada.
3.	Retornar mensagem formatada.

Pergunta: Se um cliente disser “posso falar com alguém amanhã cedo?”, o que o chatbot precisa compreender para responder bem?

________________________________________

Arquitetura de um Chatbot
Componentes principais
Componente	Função
Interface	Onde o usuário conversa (site, WhatsApp, app).
Motor de NLP	Interpreta texto e extrai intenções.
Base de conhecimento	Onde as respostas e regras estão armazenadas.
Lógica de diálogo	Controla o fluxo da conversa.
Integração/API	Conecta o chatbot a sistemas externos.

Analogia:
Pense no chatbot como um garçom digital:
•	O cliente (usuário) faz o pedido (mensagem).
•	O garçom entende o que foi pedido (intenção).
•	Consulta a cozinha (base de dados).
•	E traz a resposta (output).
Se o garçom não entender o pedido, ele pede para o cliente repetir — isso é a lógica de fallback nos chatbots.

Exemplo Conceitual
Usuário → Interface (Telegram)
         → NLP (detectar intenção)
         → Lógica (decidir resposta)
         → Base de dados (buscar informação)
         → Interface → Resposta exibida





Exercício guiado
Tarefa: desenhar o fluxograma de um chatbot que:
•	Cumprimenta o usuário;
•	Pergunta o motivo do contato;
•	Oferece 3 opções (ex.: “Horário de atendimento”, “Suporte técnico”, “Falar com humano”);
•	Responde de acordo com a escolha.
Entenda que a estrutura lógica é o primeiro passo antes do código.
________________________________________
Criando Primeiro Chatbot  - Base da lógica conversacional
Chatbot baseado em regras simples, usando condicionais if/elif.

Exemplo prático em Python
# chatbot_Um.py
def chatbot():
    print("Olá! Eu sou o seu assistente")
    while True:
        user = input("Você: ").lower()
        
        if "oi" in user or "olá" in user:
            print("Bot: Como posso te ajudar?")
        elif "horário" in user or "atendimento" in user:
            print("Bot: Nosso atendimento é das 8h às 18h, de segunda a sexta-feira.")
        elif "obrigado" in user:
            print("Bot: Fico feliz em ajudar!")
        elif "sair" in user or "tchau" in user:
            print("Bot: Até logo!")
            break
        else:
            print("Bot: Desculpe, não entendi. Pode repetir?")
chatbot()

Explicação da estrutura
•	Entrada: input() captura o que o usuário digita.
•	Processamento: o programa verifica se certas palavras estão presentes.
•	Saída: imprime a resposta correspondente.
•	Laço: mantém a conversa até o usuário dizer “tchau” ou “sair”.

Exercício prático
Tarefa: Adicione novos fluxos:
•	Quando o usuário falar “suporte”, o bot deve pedir o número do protocolo.
•	Quando o usuário falar “ajuda”, o bot deve listar as opções disponíveis.
________________________________________
Fluxos Condicionais mais complexos
Podemos criar conversas em etapas.
Por exemplo, o bot pergunta o tipo de serviço que o cliente deseja, e a resposta leva a outro conjunto de perguntas. Exemplo:
print("Olá! Eu sou o assistente virtual da UniFECAF Store.")
print("O que você deseja fazer?")
print("1 - Saber preços")
print("2 - Falar com o suporte")
print("3 - Sair")

opcao = input("Escolha uma opção: ")

if opcao == "1":
    produto = input("Qual produto deseja saber o preço? ")
    print(f"O preço do {produto} é R$ 250,00.")

elif opcao == "2":
    problema = input("Por favor, descreva o problema: ")
    print("Obrigado! Nosso suporte entrará em contato sobre:", problema)
elif opcao == "3":
    print("Até logo!")
else:
    print("Opção inválida. Tente novamente.")

Pense nesse chatbot como um menu de restaurante digital. Você escolhe uma opção e o bot te leva para o “prato” correspondente.
________________________________________

Estruturando Dados em Dicionários (Base Simples de FAQ)
Vamos evoluir a ideia usando um dicionário de perguntas e respostas — uma forma de armazenar várias combinações sem precisar criar muitos if/else.
Exemplo:
faq = {
    "horário": "Atendemos de segunda a sexta, das 8h às 18h.",
    "endereço": "Estamos localizados na Av. Paulista, 1000 - São Paulo.",
    "pagamento": "Aceitamos cartões, Pix e boleto bancário.",
    "entrega": "Realizamos entregas em todo o Brasil via correios."
}
print("Olá! Sou o Chatbot de Atendimento")
mensagem = input("Qual é sua dúvida? ")

encontrou = False
for chave in faq:
    if chave in mensagem.lower():
        print(faq[chave])
        encontrou = True
        break

if not encontrou:
    print("Desculpe, não encontrei essa informação. Deseja falar com um atendente?")
Explicação:
•	O dicionário faq guarda perguntas (chaves) e respostas (valores).
•	O loop percorre cada chave e procura na mensagem do usuário.
•	Se encontra, responde automaticamente.
________________________________________
Chatbot de FAQ Interativo
Objetivo: Construir um chatbot simples que responde perguntas frequentes e simula um pequeno atendimento virtual.
________________________________________
Etapa 1: Estrutura Base
faq = {
    "horário": "Atendemos de segunda a sexta, das 8h às 18h.",
    "produtos": "Temos Camisetas, Bonés e acessórios.",
    "pagamento": "Aceitamos cartões, Pix e boleto.",
    "entrega": "Entregamos para todo o Brasil!",
    "suporte": "Nosso suporte funciona 24h via chat."
}

print("Bem-vindo ao Assistente Virtual da UniFECAF Store")
print("Digite 'sair' para encerrar.")

while True:
    mensagem = input("Você: ").lower()
    
    if mensagem == "sair":
        print("Chatbot: Foi um prazer te atender. Até logo!")
        break
    
    respondeu = False
    for chave in faq:
        if chave in mensagem:
            print("Chatbot:", faq[chave])
            respondeu = True
            break
    
    if not respondeu:
        print("Chatbot: Desculpe, ainda não sei responder isso.")

Etapa 2: Tornando o Bot mais “Humano”
Adicione respostas aleatórias para quando não entender a mensagem:
import random
respostas_padrao = [
    "Desculpe, ainda estou aprendendo sobre isso.",
    "Pode reformular a pergunta?",
    "Hmm... acho que não tenho essa informação ainda."
]
Substitua a última linha do if not respondeu: por:
print("Chatbot:", random.choice(respostas_padrao))

Resultado: O bot agora tem mais “personalidade” e evita respostas repetitivas.
________________________________________
Exercício Prático
Exercício 1:
Adicione mais perguntas e respostas ao dicionário faq (pelo menos 5 novas).
Exercício 2:
Faça o bot perguntar o nome do usuário no início da conversa e usar o nome nas respostas.
Dica: use f"Olá, {nome}!" para formatar strings dinamicamente.

