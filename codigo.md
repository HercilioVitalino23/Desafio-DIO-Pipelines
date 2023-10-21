
### **Código**
### Extract

Aqui utilizamos a biblioteca *pandas*, e extrair os dados que precisamos:

```
import pandas as pd

# Ler o arquivo CSV pulando as duas primeiras linhas e selecionando as colunas desejadas
file_path = "Dados_desafio.csv"
df = pd.read_csv(file_path, skiprows=2, usecols=["Papel", "ROIC", "ROE", "Cresc. Rec.5a"])

# Exibir as primeiras linhas do DataFrame resultante
print(df.head())
```
Neste caso, tivemos o seguinte retorno:
```
  Papel     ROIC     ROE Cresc. Rec.5a
0  PRIO3   19,23%  34,61%        69,23%
1  RECV3   19,43%  23,12%        52,51%
2  RPMG3  -19,09%  19,09%        52,91%
3  RPMG4  -19,09%  19,09%        52,91%
4  RAIZ4    8,30%  11,14%        45,49%
```

### Transform
Aqui eu filtrei as informações das colunas para obter os ativos que seguiam os parâmetros que eu determinei (ROE e ROIC acima de 3,24% e Crescimento de receita em 5 anos cima de 49%):

```
# Remover caracteres não numéricos e converter as colunas "ROE" e "ROIC" para valores numéricos
df["ROE"] = df["ROE"].str.replace('%', '').str.replace(',', '.').astype(float)
df["ROIC"] = df["ROIC"].str.replace('%', '').str.replace(',', '.').astype(float)
df["Cresc. Rec.5a"] = df["Cresc. Rec.5a"].str.replace('%', '').str.replace(',', '.').astype(float)


# Filtrar as linhas onde o valor de "ROE" e "ROIC" seja maior que 3,24%
filtered_df = df[(df["ROE"] > 3.24) & (df["ROIC"] > 3.24) & (df["Cresc. Rec.5a"] > 49)]

# Exibir as linhas filtradas
print(filtered_df)
```
O código retornou o seguinte:
```
   Papel   ROIC    ROE  Cresc. Rec.5a
0  PRIO3  19.23  34.61          69.23
1  RECV3  19.43  23.12          52.51
```
### Load

Vamos salvar o resultado em *.csv*:
```
# Salvando os dados em .csv
filtered_df.to_csv('Empresas com crescimento acima da Selic.csv')
```
E usar a biblioteca matplotlib, para gerar gráficos em barras, comparando os dados obtidos de cada ativo e salvando em *.png*:
```
import matplotlib.pyplot as plt

# Criando o gráfico de barras
filtered_df.plot(kind='bar', x='Papel', y='Cresc. Rec.5a')
plt.xlabel('Papel')
plt.ylabel('Crescimento da Receita em 5 anos (%)')
plt.title('Crescimento da Receita em 5 anos por Papel')
plt.xticks(rotation=45)  # Para rotacionar os rótulos do eixo x
plt.tight_layout()  # Para evitar que os rótulos do eixo x se sobreponham
plt.savefig('crescimento_receita.png')  # Salvar o gráfico como um arquivo PNG
plt.show()

filtered_df.plot(kind='bar', x='Papel', y='ROIC')
plt.xlabel('Papel')
plt.ylabel('ROIC (%)')
plt.title('ROIC por Papel')
plt.xticks(rotation=45)  # Para rotacionar os rótulos do eixo x
plt.tight_layout()  # Para evitar que os rótulos do eixo x se sobreponham
plt.savefig('ROIC.png')  # Salvar o gráfico como um arquivo PNG
plt.show()

filtered_df.plot(kind='bar', x='Papel', y='ROE')
plt.xlabel('Papel')
plt.ylabel('ROE (%)')
plt.title('ROE por Papel')
plt.xticks(rotation=45)  # Para rotacionar os rótulos do eixo x
plt.tight_layout()  # Para evitar que os rótulos do eixo x se sobreponham
plt.savefig('ROE.png')  # Salvar o gráfico como um arquivo PNG
plt.show()
```
E tivemos os gráficos:
```
![grafico_roe]("C:\Users\Pichau\Desktop\Ciencia_de_dados\Pipeline\ROE.png")

![grafico_roic]()

![grafico_Cresc_Rec_5a]()
```

Subi os arquivos no meu drive:

```
import os
from google.oauth2 import service_account
from googleapiclient.discovery import build
from googleapiclient.http import MediaFileUpload

# Caminho para o arquivo JSON de credenciais
credentials_path = 'credenciais.json'

# Escopo da API do Google Drive
SCOPES = ['https://www.googleapis.com/auth/drive.file']

arquivos = [
    {'nome_arquivo': '/content/Empresas com crescimento acima da Selic.csv', 'descricao': 'Descrição do arquivo 1'},
    {'nome_arquivo': '/content/ROE.png', 'descricao': 'Descrição do arquivo 2'},
    {'nome_arquivo': '/content/ROIC.png', 'descricao': 'Descrição do arquivo 3'},
    {'nome_arquivo': '/content/crescimento_receita.png', 'descricao': 'Descrição do arquivo 4'}
]

# Criar uma instância do serviço Google Drive
credentials = service_account.Credentials.from_service_account_file(
    credentials_path, scopes=SCOPES)
drive_service = build('drive', 'v3', credentials=credentials)

for arquivo_info in arquivos:
    file_path = arquivo_info['nome_arquivo']
    file_metadata = {
        'name': arquivo_info['nome_arquivo'],
        'description': arquivo_info['descricao']
    }
# Upload do arquivo
media = MediaFileUpload(file_path, mimetype='application/octet-stream')
file = drive_service.files().create(body=file_metadata, media_body=media, fields='id').execute()

# Imprimir o ID do arquivo carregado
print('Arquivos carregados. ID: %s' % file.get('id'))
```
