# <h1 align="center"> Espectrocolors </h1>
## Descrição do Projeto
<p align="justify"> Projeto desenvolvido para a matéria de Prática de Ciência de Dados ministrada pelo professor Dr. Leandro Nascimento Lemos. Solicitou-se realizar um projeto interdisciplinar que utilizasse a computação para resolver ou otimizar um problema cotidiano ou das práticas científicas que se desenvolveu durante o primeiro semestre do curso. Assim, o grupo optou por desenvolver uma programação de desenvolvimento de gráficos para análise de resultados de absorbância por comprimento de onda obtidos por meio de aparelhoes de Espectroscopia Eletrônica. </p>
<p align="justify"> Por meio do código serão fornecidos alguns parâmetros, como: cor, meia altura(fwhm), comprimento de onda máximo, comprimento de onda mínimo, energia e comparação de picos de diferentes amostras. Esses parâmetros são importantes para o usuário compreender as características da amostra análisadas no aparelho, facilitando a compreensão dos dados dos gráficos, visto que a cor é relativa ao ponto de maior absorbância, sendo assim, essa deve corresponder a cor complementar que é refletida e vista na solução. Já o FWHM(full width at half maximum) - largura da meia altura - é um parâmetro muito importante para compreender a dispersão das partículas no meio, e com isso observar de a solução gerou um meio mais ou menos monodisperso, por exemplo na análise de nanomateriais, em que o valor do FWHM demonstra de a a preparação da solução gerou o resultado esperado, geralmente um sistema monodisperso - partículas com tamanhos iguais ou próximos. </p>
<p align="justify"> Os comprimentos de onda máximo e mínimo são importantes para entender as regiões de maior e menor absorbância, e comparar esses dados com outras informações qualitativas ou quantitativas. Com esses dados é possível calcular a energia, sendo ela inversamente proporcional ao comprimento de onda e calculada a partir da equação: E = hc/λ, onde E é energia, h constante de Planck, c a velocidade da luz e λ o comprimento de onda. Também, é possível comparar o pico de diferentes amostras por meio de uma reta tangente entre os picos delas e pela inclinação da reta analisar alguns aspectos e diferenças das amostras. </p>
<p align="justify"> Esse programa busca auxiliar os usuários a desenvolver os gráficos e a obter os dados importantes com mais praticidade, para que se torne mais fácil fazer análises qualitativas e quantitativas das informações obtidas por meio dos Espectrofotômetros. </p>
* [Título e Capa](#Titulo-e-Imagem-de-capa)
  
Índice
=================
<!--ts-->
* [Status do Projeto](#status-do-projeto)
* [Objetivos do Projeto](#Objetivos-do-projeto)
* [Descrição do projeto](#Descrição-do-projeto)
* [Funcionalidades e Demonstração da Aplicação](#Funcionalidades-e-Demonstração-da-Aplicação)
* [Ferramentas utilizados](#Métodos-utilizados)
* [Conclusão](#Conclusão)
* [Desenvolvedores do Projeto](#Desenvolvedores-do-projeto)
* [Agradecimentos e Colaborações](#Agradecimentos-e-colaborações)
<!--te-->

### Status do projeto
<h4 align="center"> 
	Concluído
</h4>

### Descrição do projeto
# PCD---Spectroscopy-Graphics
Descrição breve do projeto.
 
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

### Ferramentas Computacionais Utilizadas
As seguintes ferramentas foram usadas na construção do projeto:
- [Python 3.9](https://www.python.org/downloads/release/python-390/)
  * `Biblioteca 1`: glob
  * `Biblioteca 2`: pandas
  * `Biblioteca 3`: numpy
  * `Biblioteca 4`: matplotlib.pyplot
  * `Biblioteca 5`: shutil
  * `Biblioteca 6`: plotly.express as px
- [Jupyter](https://nodejs.org/en/)

Descrição do seu projeto;
Funcionalidades;
Como os usuários podem utilizá-lo;
Onde os usuários podem encontrar ajuda sobre seu projeto;
Autores do projeto.
