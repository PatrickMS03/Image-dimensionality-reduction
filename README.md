# Image-dimensionality-reduction

import matplotlib.pyplot as plt
from PIL import Image

'''importar matplot para visualização e PIL para manipular imagens'''

class ReduzDimen():
  def __init__(self, imagem_selecionada):
    '''construtor recebe o caminho 'imagem selecionada' e a o image abre a imagem selecionada e convertee para rgb evitando problemas com as cores'''
    self.imagem_original = Image.open(imagem_selecionada).convert('RGB')
    self.imagem_selecionada = imagem_selecionada

  def converte_cinza(self):
    '''função que transforma a imagem em cinza pegando sua altura e largura pelo metodo .size cria uma nova imagem em branco ('L', caso não tenha o código irá esperar três valores para cada pixel em vez de um)
    que irá receber os valores de cada pixel usa o load para carregar os dados mais rápido, depois o código itera sobre cada x e y obtendo os valores de cada um colocando-os na fórmula de luminosidade e montando a imagem '''
    largura, altura = self.imagem_original.size
    imagem_cinza = Image.new('L', (largura, altura))
    pixels = self.imagem_original.load()
    for x in range(largura):
      for y in range(altura):
        [r,g,b] = pixels[x,y]
        valor = int(0.299*r + 0.587*g + 0.114*b)
        imagem_cinza.putpixel((x, y), valor)

    return imagem_cinza


  def converte_binaria(self, img_cinza, limiar = 128):
    '''função que transforma a imagem cinza em preto e branco, com a função recebendo um valor default para o limiar onde dirá se a cor será preta ou branca, no caso, se for menor que 128 será preto, e se for maior será branco, sendo
    esses valores definidos pelo '1' quando é criado a nova imagem '''
    largura, altura = img_cinza.size
    imagem_binaria = Image.new("1", (largura, altura))

    for x in range(largura):
        for y in range(altura):
            valor = img_cinza.getpixel((x, y))
            pixel = 0 if valor < limiar else 255
            imagem_binaria.putpixel((x, y), pixel)

    return imagem_binaria

def main():
 '''Colocar a função main para organizar melhor'''

 imagem_selecionada = 'sua_imagem.jpg'
 rd = ReduzDimen(imagem_selecionada)
 '''imagem_selecionada será o input para colocar o arquivo da imagem'''

 try:
   '''usar o try e o except para tratamento de possíveis erros, sendo o específico de arquivo e o outro genérico'''


   '''plt.subplot é usado para separar as imagens de maneira a ficar uma linha e três colunas, com a ordem sendo definida pelo útlimo número dentro do parentese'''
   '''plt.axis('off') é usado para retirar os eixos x e y'''
   '''plt.title dá o título da imagem'''
   '''plt.imshow mapeia os dados para cores com base no cmap '''


   'imagem original'
   plt.subplot(1, 3, 1)
   plt.imshow(rd.imagem_original)
   plt.axis('off')
   plt.title('Original')

   'imagem cinza'
   plt.subplot(1, 3, 2)
   img_cinza = rd.converte_cinza()
   plt.imshow(img_cinza, cmap= 'gray')
   '''o cmap gray instrui qu eo matplot deve deixar a imagem cinza, pois caso não seja colocado o matplot irá colocar cores aleatórias, visto que o código
   só devolve números para ele interpretar'''
   plt.axis('off')
   plt.title('Gray')

   'imagem binária'
   plt.subplot(1, 3, 3)
   img_binaria = rd.converte_binaria(img_cinza)
   plt.imshow(img_binaria)
   plt.axis('off')
   plt.title('Binary')

   plt.tight_layout()
   '''ajusta o layout'''
   plt.show()
   '''renderiza as imagens'''

 except FileNotFoundError as e:
      print(f'Ocorreu o {e}:')
 except Exception as e:
       print (f'{e}')

if __name__ == '__main__':
  main()
