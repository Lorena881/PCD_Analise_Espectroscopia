![image](https://github.com/Lorena881/PCD_Analise_Espectroscopia/assets/172424739/e6ca9dbf-861e-4b91-91fe-0691270a5773) 

# <h1 align="center"> Espectrocolors </h1>

# Descrição do Projeto
<p align="justify"> Projeto desenvolvido para a matéria de Práticas em Ciência de Dados ministrada pelo professor Dr. Leandro Nascimento Lemos. Solicitou-se realizar um projeto interdisciplinar que utilizasse a computação para resolver ou otimizar um problema cotidiano, ou das práticas científicas feitas durante o primeiro semestre do curso (Bacharelado em Ciência e Tecnologia). Assim, o grupo optou por desenvolver uma programação de desenvolvimento de gráficos para análise de resultados de absorbância por comprimento de onda obtidos por meio de aparelhos de Espectroscopia Eletrônica. Para isso, contou também com a ajuda da professora Dra. Valéria Spolon Marangoni para compreender quais parâmetros eram importantes para análise da espectroscopia e deveriam estar presentes neste projeto. </p>
<p align ="justify"> Dessa forma, por meio do código serão fornecidos alguns parâmetros, como: cor, largura meia altura(fwhm), comprimento de onda máximo, comprimento de onda mínimo, energia e comparação de picos de diferentes amostras. Esse programa busca auxiliar os usuários a desenvolver os gráficos e a obter os dados importantes com mais praticidade, para que se torne mais fácil fazer análises qualitativas e quantitativas das informações obtidas por meio dos Espectrofotômetros. </p>

## Sobre a Espectroscopia
<p align="justify"> Espectroscopia pode ser descrita como a interação da radiação eletromagnética com a matéria, ou de uma grandeza em relação à frequência ou comprimento de onda, em que são utilizados espectrofotômetros para comparar a radiação incidida e emitida. Para esse projeto foi utilizado dados do espectrofotômetro com comprimentos de onda entre o ultravioleta e o visível, que proporcionam a transição do elétron para diferentes níveis de energia dependendo da energia fornecida e emitida em cada comprimento de onda. </p>
<p align="justify"> Em análises, geralmente se busca a cor relativa ao ponto de maior absorbância, pois essa deve corresponder a cor complementar, que é refletida e vista na solução. Já o FWHM (full width at half maximum) - largura da meia altura - é um parâmetro muito importante para compreender a dispersão das partículas no meio e, com isso, observar se o meio da solução é mais ou menos monodisperso. Um sistema monodisperso indica que as partículas têm tamanhos iguais ou similares, ou seja, que estão dispersas igualmente pela amostra. Na análise de nanomateriais, por exemplo, o valor do FWHM é utilizado para analisar se a preparação da solução produziu o resultado esperado, podendo ser um sistema monodisperso ou polidisperso - partículas de tamanhos diferentes, que se aglutinaram. </p>
<p align="justify"> Os comprimentos de onda máximo e mínimo são importantes para entender as regiões de maior e menor absorbância, possibilitando a comparação desses dados com outras informações qualitativas ou quantitativas, como a utilização desses para calcular a energia. A energia é obtida a partir da equação: E = hc/λ, onde E é energia, h constante de Planck, c a velocidade da luz e λ o comprimento de onda; sendo ela inversamente proporcional ao comprimento de onda. Também é possível comparar o pico de diferentes amostras por meio de uma reta tangente entre os picos delas e pela inclinação da reta analisar alguns aspectos e diferenças das amostras. </p>
  
Índice
=================
<!--ts-->
* [Status do Projeto](#status-do-projeto)
* [Objetivos do Projeto](#Objetivos-do-projeto)
* [Funcionalidades e Demonstração da Aplicação](#Funcionalidades-e-Demonstração-da-Aplicação)
* [Ferramentas utilizados](#Métodos-utilizados)
* [Conclusão](#Conclusão)
* [Referências](#Referências)
* [Desenvolvedores do Projeto](#Desenvolvedores-do-projeto)
* [Professores](#Professores)
<!--te-->

# Status do projeto
<h4 align="center"> 
	Concluído
</h4>

# Objetivos do Projeto
* Desenvolver gráficos de absorbância por comprimento de onda
* Obter parâmetros das substâncias pela análise dos dados
* Facilitar a obtenção e compreensão dos dados pelos usuários

# Funcionalidades e Demonstração da Aplicação

## Leitura e plotagem dos arquivos
Minha ideia para o código era que fosse o mais fácil e acessível possível. Cada máquina de espectroscopia trabalha de uma maneira diferente e recebemos arquivos tanto em .xlsx quanto em .txt. A leitura desses arquivos é diferente, então defini duas funções que leem esses arquivos e coloquei dentro de uma função maior:
## Criando uma função para leitura, análise e plotagem de arquivos em txt: 
A função terá 12 argumentos, sendo que somente três são essenciais: o nome do arquivo, o eixo x e o eixo y. Os outros argumentos são parâmetros da plotagem e a definição se encontrar o máximo e o mínimo da função é necessário. Sua principal limitação é que, para a análise do ponto máximo e mínimo, é primordial que as colunas da dataframe estejam nomeadas de 'Wavelength (nm)' e 'Absorbance', o que já é comum nos documentos produzidos por máquinas de espectroscopia.
```python
def plotar_graficotxt(arquivo, x, y, marker='', color='black', label='', figsize=(12,8), xlabel='', ylabel='', title='', maximo=True, minimo=True):
```
Acima, estão os doze parâmetros da função: o arquivo escolhido, o eixo x, o eixo y, a escolha de marcador da linha, a cor da linha, o nome da linha, o tamanho da figura, o nome para o eixo x, o nome para o eixo y, o título do gráfico, se precisa achar o máximo e se precisa encontrar o mínimo. 
Notaremos que o arquivo não precisa ser nomeado como '*.txt', o que facilita o usuário.
```python
    import matplotlib.pyplot as plt
    import pandas as pd
    import numpy as np
    cor_maximo = ''
    cor_minimo = ''
```
Agora, definiremos um dicionário básico para a escolha das cores.
```python
    cores = {range(625, 741): "vermelho",
    range(590, 626): "laranja",
    range(565, 591): "amarelo",
    range(500, 566): "verde",
    range(440, 486): "azul",
    range(380, 441): "violeta"}
```
Começaremos agora a leitura do arquivo
```python
    arquivo = f"{arquivo}.txt"
    df = pd.read_csv(arquivo)
```
Para a análise do ponto máximo e mínimo, tive que definir um valor mínimo e máximo, pois existem limitações na leitura pela máquina.
A função organiza a coluna de dados de maneira descendente para encontrar o ponto máximo nos eixos x e y; e de maneira ascendente para encontrar o ponto mínimo nos eixos x e y. Ela então pega o primeiro elemento:
```python
    ponto_maximox, ponto_maximoy = df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=False).iloc[0, 0], df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=False).iloc[0, 1]
    ponto_minimox, ponto_minimoy = df.loc[df['Wavelength (nm)'] >= 240 & df['Wavelength (nm)' <= 1200].sort_values(by='Absorbance', ascending=True).iloc[0, 0], df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=True).iloc[0, 1]
```
Plotagem do gráfico. Além de plotar, defini que, se o usuário quiser o ponto máximo e mínimo, marcaremos ele no gráfico.
A plotagem do gráfico é padrão:
```python
    ax = df.plot(x=x, y=y, marker=marker, color=color, label=label, figsize=figsize)
    plt.xlabel(xlabel)
    plt.ylabel(ylabel)
    plt.title(title)
```
A condição de máximo e mínimo está aqui:
```python
    if maximo == True:
        plt.scatter(ponto_maximox, ponto_maximoy, color='red', label='Ponto máximo')
        plt.text(ponto_maximox, ponto_maximoy, f'({ponto_maximox:.2f}, {ponto_maximoy:.2f})', fontsize=10, ha='center', va='bottom', color='black')
        for faixa, cor in cores.items():
            if ponto_maximox in faixa:
                print('O ponto máximo de absorção da luz pela solução ocorre na cor ', cor)
            break 
    if minimo == True:
        plt.scatter(ponto_minimox, ponto_minimoy, color='blue', label='Ponto mínimo')
        plt.text(ponto_minimox, ponto_minimoy, f'({ponto_minimox:.2f}, {ponto_minimoy:.2f})', fontsize=10, ha='center', va='top', color='black')
        for faixa, cor in cores.items():
            if ponto_minimox in faixa:
                print('O ponto mínimo de absorção da luz pela solução ocorre na cor ', cor)
                break
```
Por fim, selecionei a legenda por questões estéticas e pedi para exibir.
```python
    plt.legend(loc='best', fontsize='medium', frameon=True, shadow=True)
    plt.show()
```
## Criando uma função para os arquivos em xlsx:
A estrutura do código não muda muito em relação ao código anterior. Por isso, comentarei somente a parte modificada.
```python
def plotar_graficoxlsx(arquivo, x, y, marker='', color='black', label='', figsize=(12,8), xlabel='', ylabel='', title='', maximo=True, minimo=True):
```
Definidos o nome e os argumentos da função, o código segue:
```python
    import matplotlib.pyplot as plt
    import pandas as pd
    import numpy as np
    cores = {range(625, 741): "vermelho",
    range(590, 626): "laranja",
    range(565, 591): "amarelo",
    range(500, 566): "verde",
    range(440, 486): "azul",
    range(380, 441): "violeta"}
```
A alteração foi aqui: transformei a string 'arquivo' em uma string 'arquivo.xlsx', e a li pelo read_xlsx.
arquivo = f"{arquivo}.xlsx"
```python
    df = pd.read_xlsx(arquivo)
    ponto_maximox, ponto_maximoy = df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=False).iloc[0, 0], df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=False).iloc[0, 1]
    ponto_minimox, ponto_minimoy = df.loc[df['Wavelength (nm)'] >= 240 & df['Wavelength (nm)' <= 1200].sort_values(by='Absorbance', ascending=True).iloc[0, 0], df.loc[df['Wavelength (nm)'] >= 240].sort_values(by='Absorbance', ascending=True).iloc[0, 1]
    ax = df.plot(x=x, y=y, marker=marker, color=color, label=label, figsize=figsize)
    plt.xlabel(xlabel)
    plt.ylabel(ylabel)
    plt.title(title)
    if maximo == True:
        plt.scatter(ponto_maximox, ponto_maximoy, color='red', label='Ponto máximo')
        plt.text(ponto_maximox, ponto_maximoy, f'({ponto_maximox:.2f}, {ponto_maximoy:.2f})', fontsize=10, ha='center', va='bottom', color='black')
        for faixa, cor in cores.items():
            if ponto_maximox in faixa:
                print('O ponto máximo de absorção da luz pela solução ocorre na cor ', cor)
            break 
    if minimo == True:
        plt.scatter(ponto_minimox, ponto_minimoy, color='blue', label='Ponto mínimo')
        plt.text(ponto_minimox, ponto_minimoy, f'({ponto_minimox:.2f}, {ponto_minimoy:.2f})', fontsize=10, ha='center', va='top', color='black')
        for faixa, cor in cores.items():
            if ponto_minimox in faixa:
                print('O ponto mínimo de absorção da luz pela solução ocorre na cor ', cor)
                break
    plt.legend(loc='best', fontsize='medium', frameon=True, shadow=True)
    plt.show()
```
## Escrevendo a função principal
Para a função principal, não foi necessário muita alteração. A função, então, lê o arquivo e escolhe se usaremos a plotagem para xlsx ou para txt. Ela recebe os mesmos argumentos que as outras duas funções, e, ao chamar a função de plotagem, escolhe os mesmos argumentos.
 
```python
def plot_grafico(arquivo, x, y, marker='', color='red', label='', figsize=(12,8), xlabel ='', ylabel = '', title = '', maximo = True, minimo = True):
    import os
    from glob import glob
    directory = os.getcwd()
    directory_txt = f"{directory}\\*.txt"
    directory_xlsx = f"{directory}\\*.xlsx"
    list_txt = [i for i in glob(directory_txt)]
    list_xlsx = [i for i in glob(directory_xlsx)]
```
A estrutura montada realmente visa a maior facilidade ao usuário. Para solucionar o problema de não saber se o arquivo é .txt ou .xlsx, utilizei o glob para pegar todos os arquivos do mesmo tipo no diretório selecionado. Caso esteja lá, ele encaminhará para a análise correspondente. Caso não esteja, ele retornará dizendo que não encontrou o arquivo.
```python
    arquivo_txt = f"{directory}\\{arquivo}.txt"
    arquivo_xlsx = f"{directory}\\{arquivo}.xlsx"
    if arquivo_txt in list_txt:
        plotar_graficotxt(arquivo, x, y, marker,color,label,figsize,xlabel,ylabel,title, maximo, minimo)
    elif arquivo_xlsx in list_xlsx:
        plotar_graficoxlsx(arquivo, x, y, marker,color,label,figsize,xlabel,ylabel,title, maximo, minimo)
    else:
        raise ValueError('Tipo de arquivo não suportado. Use .txt ou .xlsx')
## FWHM
Primeiro selecione o arquivo de dados escolhido e defina as bandas para achar o pico definido. Essa parte do código é customizada.
 
```python
df = pd.read_csv("seus_dados.txt")
df = df[(df["Wavelength (nm)"] >= 300) & (df["Wavelength (nm)"] <= 500)]
```
A parte a seguir é inalterável, ela define as linhas de mínino e máximo dos pontos do pico escolhido e calcula a largura à meia altura.
 
```python
max_val = df2["Absorvância"].max()
min_val = df2["Absorvância"].min()
 
meia_altura = (max_val - min_val)/2
meia_altura_abs = df.iloc[(df["Absorvância"] - meia_altura).abs().argsort()[:2]]
fwhm = (meia_altura_abs.iloc[0] - meia_altura_abs.iloc[1])["Wavelength (nm)"]
print(fwhm)
```
FWHM (full width half maximum/largura à meia onda) é uma métrica utilizada para a caracterização e análise de dados experimentais em várias técnicas químicas. Em espectroscopia de UV-Vis é usada para caracterizar a largura dos picos de absorção, que pode fornecer informações sobre a pureza da amostra e sobre a interação entre as moléculas.

## Energia
Primeiro, foi feita uma função para determinar o comprimento de onda máximo:
``` python
# define o ponto de maior absorção 
def absorcao_maxima(arquivo):
  # Lê o arquivo e encontra o valor da absorção máxima dentro da coluna de Absorbância
    df = pd.read_csv(arquivo) 
    maior_absorcao = df["Absorbance"].max()
    return maior_absorcao 
```
Assim, utiliza-se o valor de absorção máxima para calcular a energia por meio da equação(derivada da equação de Schrödinger):
```python
def energia(arquivo):
    df = pd.read_csv(arquivo)
    linha = df[df['Absorbance'] == absorcao_maxima(arquivo)]
    comprimento_maximo = linha.iloc[0]['Wavelength (nm)']
    comprimento = comprimento_maximo * (10**-9)  # Convertendo nm para metros
    energia = ((6.63 * 10**-34) * (3 * 10**8)) / comprimento
    return energia
```
A energia em relação ao comprimento de onda fornece o valor da energia do sistema, em Joules(J). Essa energia é a energia de ionização, ou de emissão em relação ao comprimento de onda, em que, geralmente, é calculada para o comprimento de onda da absorbância máxima, mas também pode ser utilizada para determinar a energia em um comprimento de onda específico.

## Picos de absorção traçados por uma tangente
Primeiro utilizamos três funções: uma para ler o arquivo .txt, outra para normalizar a absorção que pode ter diferentes escalas e no fim para tirarmos os pontos máximos que irão ser comparados.
 
```python
# Ler os dados do arquivo .txt
def ler_dados(arquivo_txt):
    # Ler o arquivo .txt em um DataFrame, supondo que as colunas são separadas por espaços
    df = pd.read_csv(arquivo_txt, sep='\s+', header=None)
    return df
 
# Normalizar a absorção
def normalizar_coluna(df, coluna_index, valor_max):
    df.iloc[:, coluna_index] = (df.iloc[:, coluna_index] / df.iloc[:, coluna_index].max()) * valor_max
    return df
 
# Encontrar os pontos máximos
def encontrar_ponto_maximo(df):
    indice_maximo = df.iloc[:, 1].idxmax()
    x_max = df.iloc[indice_maximo, 0]
    y_max = df.iloc[indice_maximo, 1]
    return x_max, y_max
```
A parte a seguir é alterável, ela puxa os arquivos do seu diretório. Você pode usar quantos arquivos quiser.
 
```python
# Caminhos para os arquivos .txt
arquivo1 = 'seu_arquivo1.txt'  
arquivo2 = 'seu_arquivo2.txt'   
 
# Ler os dados dos arquivos
df1 = ler_dados(arquivo1)
df2 = ler_dados(arquivo2)
 
# Normalizar a segunda coluna do primeiro DataFrame. Aqui usamos um exemplo para que o maior valor seja 1.4
df1 = normalizar_coluna(df1, 1, 1.4)
df2 - normalizar_coluna(df3, 1, 1.4)
```
Agora basta criar o gráfico e ligar os pontos:
 
```python
# Criar o gráfico
plt.figure(figsize=(10, 6))
 
# Plotar o primeiro arquivo normalizado
plt.plot(df1.iloc[:, 0], df1.iloc[:, 1], label='Extinção Ideal para 63 nm', c = '#800000') #plota para o arquivo a primeira coluna como eixo X (comprimento de onda) e a segunda como eixo Y (absorbância)
x_max1, y_max1 = encontrar_ponto_maximo(df1) #define qual é o ponto máximo do pico chamando a função no arquivo
plt.scatter(x_max1, y_max1, color='red', label=f'Ponto Máximo Tabela 1: ({x_max1} nm)') #plota o ponto máximo que será comparado
 
# Plotar o segundo arquivo normalizado
plt.plot(df2.iloc[:, 0], df2.iloc[:, 1], label='Extinção da Síntese II', c = 'orange') #plota para o arquivo a primeira coluna como eixo X (comprimento de onda) e a segunda como eixo Y (absorbância)
x_max2, y_max2 = encontrar_ponto_maximo(df2) #define qual é o ponto máximo do pico chamando a função no arquivo
plt.scatter(x_max2, y_max2, color='red', label=f'Ponto Máximo da Síntese II: ({x_max2} nm)') #plota o ponto máximo que será comparado
```
Por fim use plt.plot nos pontos encontrados para conectar os pontos.
```python
plt.plot([x_max1, x_max2], [y_max1, y_max2], linestyle='--', color='black')
plt.plot([x_max2, x_max3], [y_max2, y_max3], linestyle='--', color='black')
 
# Exibir o gráfico
plt.tight_layout()
plt.show()
```
A comparação dos picos de máxima absorção é uma prática central na química analítica e em várias outras disciplinas químicas, sendo crucial para a identificação de compostos, quantificação, monitoramento de reações e estudo de interações moleculares. Por exemplo, os espectros obtidos pela espectroscopia UV-Vis apresentam picos característicos para diferentes compostos. Comparar esses picos de absorção máxima permite identificar quais compostos estão presentes na amostra. Cada composto absorve luz em comprimentos de onda específicos, resultando em picos de absorção distintos. Utilizamos desse conhecimento muitas vezes durante as aulas de laboratório do primeiro semeste.

## Gráficos interativos
É possível tornar os gráficos de absorção por comprimento de onda interativos em que cada ponto mostra as informações de comprimento de onda e de absorção, além de mostrar a cor do espectro de cada intervalo.

```python
import plotly.express as px
import plotly.graph_objects as go
import pandas as pd
import numpy as np

def plotar_grafico(arquivo, x, y, marker='', label='', figsize=(12, 8), xlabel='', ylabel='', title='', maximo=True, minimo=True):
    cores = {
        range(625, 741): "red",
        range(590, 626): "orange",
        range(565, 591): "yellow",
        range(492, 566): "green",
        range(440, 492): "blue",
        range(380, 441): "violet"
    }
    
    # Detectar o tipo de arquivo
    if arquivo.endswith('.txt'):
        df = pd.read_csv(arquivo)
    elif arquivo.endswith('.xlsx'):
        df = pd.read_excel(arquivo)
    else:
        raise ValueError("Formato não suportado. Use .txt ou .xlsx")

    # Filtrar os dados para incluir apenas o espectro de luz visível
    df = df.loc[(df['Wavelength (nm)'] >= 400) & (df['Wavelength (nm)'] <= 700)]
    
    # Encontrar pontos máximo e mínimo
    ponto_maximo = df.loc[df['Absorbance'].idxmax()]
    ponto_minimo = df.loc[df['Absorbance'].idxmin()]

    # Criar o gráfico interativo
    fig = go.Figure()

    # Segmentar os dados e adicionar cada segmento como uma linha com a cor correspondente
    for faixa, cor in cores.items():
        segment = df[df[x].between(faixa.start, faixa.stop, inclusive='left')]
        if not segment.empty:
            fig.add_trace(go.Scatter(x=segment[x], y=segment[y], mode='lines+markers', line=dict(color=cor), name=f"{faixa.start}-{faixa.stop} nm"))

    # Adicionar pontos máximo e mínimo
    if maximo:
        fig.add_trace(go.Scatter(x=[ponto_maximo[x]], y=[ponto_maximo[y]], mode='markers+text', text=[f'Máximo: ({ponto_maximo[x]:.2f}, {ponto_maximo[y]:.2f})'], textposition='top center', marker=dict(color='red', size=10), name='Ponto máximo'))
        for faixa, cor in cores.items():
            if ponto_maximo[x] in faixa:
                print('O ponto máximo de absorção da luz pela solução ocorre na cor ', cor)
                break

    if minimo:
        fig.add_trace(go.Scatter(x=[ponto_minimo[x]], y=[ponto_minimo[y]], mode='markers+text', text=[f'Mínimo: ({ponto_minimo[x]:.2f}, {ponto_minimo[y]:.2f})'], textposition='bottom center', marker=dict(color='blue', size=10), name='Ponto mínimo'))
        for faixa, cor in cores.items():
            if ponto_minimo[x] in faixa:
                print('O ponto mínimo de absorção da luz pela solução ocorre na cor ', cor)
                break
    
    fig.update_layout(title=title, xaxis_title=xlabel, yaxis_title=ylabel, width=figsize[0]*100, height=figsize[1]*100)
    
    fig.show()

# Exemplo de chamada da função
plotar_grafico('Lorena_Emanuel_Azul.txt', x='Wavelength (nm)', y='Absorbance', xlabel='Comprimento de Onda (nm)', ylabel='Absorbância', title='Espectro de Absorbância')
```
Para isso, foi utilizado a base do gráfico clássico juntamente com as alterações, mostradas no código acima, para a biblioteca de desenvolvimento de gráficos interativos, plotly.


# Ferramentas Computacionais Utilizadas
As seguintes ferramentas foram usadas na construção do projeto:
- [Python 3.9](https://www.python.org/downloads/release/python-390/)
  * `Biblioteca 1`: glob
  * `Biblioteca 2`: pandas
  * `Biblioteca 3`: numpy
  * `Biblioteca 4`: matplotlib.pyplot
  * `Biblioteca 5`: shutil
  * `Biblioteca 6`: plotly.express 
- [Jupyter](https://nodejs.org/en/)

# Conclusão
Portanto, esse projeto utilizou de algumas bibliotecas do Python, dentre elas: pandas, matplotlib.pyplot e plotlyexpress foram utilizadas para desenvolver os aspectos gráficos; numpy e shutil para as operações; os e glob para adequação do diretório. Assim, foi utilizado de alguns recursos dessas para desenvolver o código, para que fosse possível obter gráficos de linha interativos, o que permite interagir diretamente com o gráfico e obter alguns informações diretamente neles, como a cor de cada comprimento, tornando a compreensão muito mais fácil e dinâmica. Assim, por meio de arquivos padrão das Espectrocopias, conseguiu-se adicionar todos os principais parâmetros que eram visados, além tê-los tornados mais didáticos, com gráficos com cores, pontos de máximo e mínimo com informações sobre comprimento e absorção, e comparações de picos. 

### Referências


### Desenvolvedores do Projeto
| [<img loading="lazy" src="https://github.com/Lorena881/PCD_Analise_Espectroscopia/assets/172424739/d7a1d027-4bfb-4b1e-81b4-5ca44b1e3abc" width=115><br><sub>Sophia Nascimento Silva</sub>](https://github.com/sophianascto) |  [<img loading="lazy" src="https://avatars.githubusercontent.com/u/172425615?v=4" width= 115><br><sub>Henrique Valente Nogueira </sub>](https://github.com/henriquevalentenogueira) |  [<img loading="lazy" src= "https://github.com/Lorena881/PCD_Analise_Espectroscopia/assets/172424739/89c84357-c055-4c17-914e-e179974e38d5" width=115><br><sub>Lorena Ribeiro Nascimento </sub>](https://github.com/Lorena881) |
| :---: | :---: | :---: |

### Professores

| [<img loading="lazy" src="https://avatars.githubusercontent.com/u/1894434?v=4" width=115><br><sub>Leandro Nascimento Lemos</sub>](https://github.com/llemos) <p><sub>[Lattes](https://buscatextua.l.cnpq.br/buscatextual/visualizacv.do)   </sub></p> | [<img loading="lazy" src="http://servicosweb.cnpq.br/wspessoa/servletrecuperafoto?tipo=1&id=K4252367Y6" width=85><br><sub> Valéria Spolon Marangoni </sub>](https://buscatextual.cnpq.br/buscatextual/visualizacv.do?metodo=apresentar&id=K4252367Y6) <p><sub>[Lattes](https://buscatextual.cnpq.br/buscatextual/visualizacv.do?metodo=apresentar&id=K4252367Y6) </p></sub>||
| :---: | :---: | :---: |

<img loading="lazy" src="https://github.com/Lorena881/PCD_Analise_Espectroscopia/assets/172424739/c930826b-3189-41d5-b4cc-a33dbf3ee611"> 
