import requests
import time
import json
import os
class TelegramBot:
    def __init__(self):
        iTOKEN  = '5788692435:AAEVxunCKigi5oPp6tnYbT9mBXLF1QlVPsw'
        self.iURL = f'https://api.telegram.org/bot{iTOKEN}/'

    def Iniciar(self):
        iUPDATE_ID = None
        while True:
            iATUALIZACAO = self.ler_novas_mensagens(iUPDATE_ID)
            IDADOS = iATUALIZACAO["result"]
            if IDADOS:
                for dado in IDADOS:
                    iUPDATE_ID = dado['update_id']
                    mensagem = str(dado["message"]["text"])
                    chat_id = dado["message"]["from"]["id"]
                    primeira_mensagem = int(dado["message"]["message_id"]) == 1
                    resposta = self.gerar_respostas(mensagem, primeira_mensagem)
                    self.responder(resposta, chat_id)

    def ler_novas_mensagens(self, iUPDATE_ID):
        iLINK_REQ = f'{self.iURL}getUpdates?timeout=5'
        if iUPDATE_ID:
            iLINK_REQ = f'{iLINK_REQ}&offset={iUPDATE_ID + 1}'
        iRESULT = requests.get(iLINK_REQ)
        return json.loads(iRESULT.content)

    def gerar_respostas(self, mensagem, primeira_mensagem):
        print('mensagem do cliente: ' + str(mensagem))
        if primeira_mensagem == True or mensagem.lower() in ("acesse o menu  de escolha  de celular para seu  dia "):
            return f'''Olá seja bem vindo  eu sou Ruby seu assistente virtual que vai te ajudar a escolher o melhor celular para seu dia !:{os.linesep}1 - digite 1 para escolher  o melhor celular para você trabalhar {os.linesep}2 -digite 2 para escolher o melhor celular para estudar{os.linesep}3 - digite 3 para escolher o melhor celular para você jogar '''
        if mensagem == '1':
            return f'''o melhor celular para você é trabalhar é Smartphone Samsung Galaxy A21s."
                       "as especificaçao são Display: 6.5 com Resolução: 720 x 1600px  Sistema Operacional: Android 10 atualização para android12"
                       "Capacidade: 32 GB 3 GB RAM, 64 GB e versão de 128GB  4/6 GB RAM o valor é de   R$ 1.370 mais pode variar.
            '''
        elif mensagem == '2':
            return f'''"o melhor celular para você estudar é Smartphone Samsung Galaxy A13 as. especificação Possui tela com resolução Full HD"
                        "Processador Exynos 850 tem bom desempenho Traz 128 GB de armazenamento interno Já chega com Android 12 de fábrica"
                        "Bateria duradouraTem conjunto triplo de câmeras e sensor de 50 MP . o valor é 999,00")
            '''
        elif mensagem == '3':
            return f'''o melhor celular para jogar é Smartphone Samsung Galaxy M23 5G  as especificaçoes sao O Galaxy M23 da Smasung é um dos melhores custo-benefício da categoria, pensando em celular bom para jogos e barato. A tela de 6.6"
                       "plegadas com resolução Full HD+ garante boa nitidez, enquanto a taxa de atualização de 120 Hz ajuda a suavizar as animações. Ainda mais aliado a um processador Snapdragon 750G, com 6GB de memória RAM e 128GB de memória interna.
                       o valor é R$ 1.399,00 mais pode variar '''
        else:
            return 'digite menu para escolher o melhor celular para o seu dia ! "'

    def responder(self, resposta, chat_id):
        iLINK_REQ = f'{self.iURL}sendMessage?chat_id={chat_id}&text={resposta}' 
        requests.get(iLINK_REQ)
        print("respondi: " + str(resposta))


bot = TelegramBot()
bot.Iniciar() 
 
