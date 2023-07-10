Código teste calendario.
Codigo criado com o objetivo de concatenar as informações da coluna de datas com a coluna de nomes 
# Importando Bibliotecas
from datetime import datetime
import pandas as pd
import calendar

# Baixando planilha em xlsx
df = pd.read_excel('teste.xlsx')

print(df.columns)

#Selecionando colunas nome e data
df_nomedata = df[['Engineer Eta',  'Assigned Eng']]  

# planilha nome data
print(df_nomedata)

# Dicionário vazio
agendamento = {}

# Seleção linhas planilha
for index, row in df_nomedata.iterrows():
    nome = row['Assigned Eng']
    data = row['Engineer Eta']
    
    # Check nome e a data não são nulos
    if pd.notnull(nome) and pd.notnull(data):
      
        # Associando nome e data da linha a chave
        agendamento[nome] = {
            'Engineer Eta': data,
            'Assigned Eng': nome }


print(agendamento)


def imprimir_calendario(agendamento):
    # Obter o ano, mês, dia, hora, minuto e segundo atuais
    agora = datetime.now()
    ano_atual = agora.year
    mes_atual = agora.month

    # Gerar o calendário do mês atual
    calendario = calendar.monthcalendar(ano_atual, mes_atual)

    # Percorrer cada semana do calendário
    for semana in calendario:
        linha_calendario = []
        for dia in semana:
            if dia == 0:
                # Se o dia for 0, não é um dia válido no mês atual
                linha_calendario.append(' ')
            else:
                # Converter o dia para uma string
                dia_str = str(dia)

                # Check de há um evento agendado nesse dia
                eventos = []
                for nome, info in agendamento.items():
                    data_str = info['Engineer Eta']
                    data_evento = datetime.strptime(data_str, '%Y-%m-%d %H:%M:%S')
                    if dia == data_evento.day:
                        hora_evento = data_evento.strftime('%H:%M:%S')
                        eventos.append(f'{nome} ({hora_evento})')

                # concatenar o dia e os eventos agendados
                eventos_str = '\n'.join(eventos)
                linha_calendario.append(f'{dia_str}\n{eventos_str}')

        # Imprimir a linha do calendário
        print(' | '.join(linha_calendario))

# print calendario
imprimir_calendario(agendamento)
