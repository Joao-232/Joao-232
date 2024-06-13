import numpy as np
import matplotlib.pyplot as plt

n = 1000  # número de células

# Definir uma função para o modelo de célula infectada ou não
def inicial_celula(numero_celula, infectado):
    celu = np.random.choice([0, 1], (n, n), p=(1 - infectado, infectado))
    return celu

# Atualizar a vizinhança
def atualizar_vizinhaca(celula, vizinha):
#Atualiza o estado de uma célula com base em seus vizinhos.

#Args:
  #celula: Estado atual da célula (0 = saudável, 1 = infectado).
  #Vizinha: Array NumPy com os estados dos vizinhos da célula.

#Returns:
  #Estado futuro da célula (0 = saudável, 1 = infectado).
  # Regra de transição
  if celula == 1:
    #Regra para recuperação
    if np.sum(vizinha) == 0:
        return 0
    else:
        # Regra para morte
        if np.random.random() < taxa_de_morte:
            return 0
        else:
            return 1
  else:
    # Regra para infecção
    if np.sum(vizinha) >= limite_de_infecao:
        return 0
    else:
        return 1

def simulacao(celu, passos):
  #Passos:Números

  lista = []
  for _ in range(passos):
    new_celu = np.copy(celu)
    for i in range(celu.shape[0]):
      for j in range(celu.shape[1]):
        vizinha = celu[i - 1 : i + 2, j - 1 : j + 2].flatten()
        vizinha = celu[i, j]
        new_celu[i, j] = atualizar_vizinhaca(celu[i, j], vizinha)
    celu = new_celu
    lista.append(celu.copy())
  return lista

def grafico_resultado(lista):
  numero_infectado = []
  for celu in lista:
    numero_infectado.append(np.sum(celu == 1))

  plt.plot(range(len(numero_infectado)), numero_infectado)
  plt.xlabel("Tempo")
  plt.ylabel("Número de indivíduos infectados")
  plt.title("Evolução da epidemia")
  plt.show()
# Parâmetros
taxa_de_morte = 0.6
limite_de_infecao = 0.2

# Executar a simulação
celu_inicial = inicial_celula(n, infectado=0.1)
lista_resultados = simulacao(celu_inicial, passos=100)
grafico_resultado(lista_resultados)
