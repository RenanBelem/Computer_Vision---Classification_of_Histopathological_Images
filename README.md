# Classificação de Imagens Histopatológicas de Câncer de Mama
> Trabalho realizado para a disciplina: Visão Computacional, no curso de Inteligência Artifical Aplicada da UFPR

Este projeto consiste em um sistema de Visão Computacional para a classificação de imagens de biópsias de câncer de mama em quatro categorias (0, 1, 2 e 3). O trabalho utiliza imagens do dataset **Warwick**, focando em técnicas de extração de características (LBP e VGG16) e modelos de Deep Learning (*Transfer Learning*).


## 📊 Dataset

* **Origem:** Recortes de imagens WSI (Whole Slide Imaging) da Universidade de Warwick.
* **Dimensões:** Imagens de  pixels.
* **Classes:** 0, 1, 2 e 3.
* **Separação de Dados:** Divisão de 80% para treino e 20% para validação, realizada **por paciente** (Patient ID) para evitar viés cognitivo do modelo.


## 🛠️ Tecnologias e Bibliotecas

* **Linguagem:** Python 
* **Deep Learning:** TensorFlow e Keras 
* **Machine Learning:** Scikit-learn 
* **Processamento de Imagem:** OpenCV, PIL e Scikit-image 
* **Manipulação de Dados:** NumPy e Pandas 


## 🚀 Metodologia

O projeto foi dividido em duas abordagens principais:

### 1. Extração de Características + Classificadores Clássicos

Foram extraídos vetores de características utilizando:

* **LBP (Local Binary Pattern):** Captura texturas locais da imagem.
* **VGG16 (como extrator):** Utilização da penúltima camada da rede pré-treinada para gerar vetores de 512 características.

Modelos treinados nesta etapa: **Random Forest, SVM e RNA (MLP)**.

### 2. Deep Learning (Transfer Learning)

Treinamento de redes neurais convolucionais adaptadas para as 4 classes do problema:

* **Modelos:** VGG16 e ResNet50.
* **Cenários:** Testes com e sem **Data Augmentation** por 10 épocas.
* **Ajuste:** Redimensionamento das imagens de  para  pixels.



## 📈 Resultados Iniciais

| Modelo / Abordagem | Acurácia de Teste |
| --- | --- |
| RNA (Extrator VGG16) | 80,59% |
| VGG16 (Sem Augmentation) | 68,46% |
| ResNet50 (Com Augmentation) | <br>**91,37%** |

> **Nota:** O melhor desempenho geral foi alcançado pela **ResNet50 com Data Augmentation**, atingindo uma acurácia de **91,37%** na base de teste.

---

## ⚙️ Como Executar

### 1. Pré-requisitos

Certifique-se de ter o **Python 3.12** instalado. O projeto foi desenvolvido originalmente em ambiente Google Colab, mas é compatível com qualquer ambiente local que suporte as bibliotecas abaixo.

### 2. Instalação de Dependências

Instale as bibliotecas necessárias utilizando o `pip`:

```bash
pip install tensorflow numpy pandas matplotlib seaborn scikit-learn scikit-image opencv-python joblib pillow

```

* **TensorFlow/Keras:** Para construção e treinamento das CNNs e extração de características via VGG16 .
* **Scikit-learn:** Para os modelos SVM, Random Forest e MLP, além das métricas de avaliação .
* **Scikit-image:** Utilizada especificamente para o cálculo do LBP (Local Binary Pattern).
* **OpenCV/Pillow:** Para manipulação e redimensionamento das imagens histológicas .


### 3. Preparação dos Dados

Datasets provenientes do Artigo Científico: "Her2 challenge contest: a detailed assessment of automated her2 scoring algorithms in whole slide images of breast cancer tissues" (Qaiser et al., 2018).
> Datasets: https://warwick.ac.uk/fac/cross_fac/tia/data/her2contest/

* Detalhes Importantes sobre o Dataset:
Acesso: O acesso aos dados (como os arquivos Train_Warwick.zip e Test_Warwick.zip mencionados no projeto) geralmente requer um registro prévio no portal do TIA Centre (Tissue Image Analytics Centre) da universidade.
* Conteúdo: O conjunto original é composto por imagens WSI (Whole Slide Imaging) de 86 casos de pacientes com carcinoma invasivo, das quais foram extraídos os recortes (patches) de $250 \times 250$ pixels utilizados no seu trabalho.

Para que o código funcione corretamente, organize os arquivos conforme a estrutura abaixo (o script lida com a descompactação automática dos arquivos `.zip`) :

* Coloque o arquivo `Train_Warwick.zip` na raiz do projeto.
* Coloque o arquivo `Test_Warwick.zip` na raiz do projeto.


### 4. Fluxo de Execução

O projeto segue uma ordem lógica de tarefas que deve ser respeitada para a geração dos arquivos intermediários (`.csv` e `.pkl`) :

1. **Extração de Características:** Execute as funções de extração para gerar os arquivos na pasta `features_csv/` .

2. **Treinamento:** Treine os modelos clássicos (Tarefa 4) e as CNNs (Tarefa 2 das Redes Neurais), o que gerará os artefatos em `trained_models/` .

3. **Avaliação:** Aplique os modelos na base de teste para calcular Sensibilidade, Especificidade e F1-Score.


---

## 📁 Estrutura Final de Arquivos Gerados

Após a execução completa, seu diretório terá esta organização:

* `features_csv/`: Contém os vetores de características extraídos e as predições finais.

* `trained_models/` ou `cnn_trained_models/`: Contém os modelos salvos em formato `.pkl` (Scikit-learn) ou `.keras` (Deep Learning).

* `artifacts_lbp.pkl` e `artifacts_vgg16.pkl`: Pesos e escalonadores dos modelos clássicos.

---

### 📊 Comparativo de F1-Score por Classe (Base de Teste)

Os resultados abaixo mostram como cada modelo lidou com as diferentes categorias de biópsia. Nota-se que a **Classe 3** e a **Classe 0** geralmente apresentam resultados superiores, enquanto as classes intermediárias (1 e 2) são mais desafiadoras.

#### 1. Modelos com Extração de Características (Tarefas 7 e 8)

Nesta abordagem, os modelos foram treinados sobre vetores pré-extraídos .

| Extrator | Modelo | Classe 0 | Classe 1 | Classe 2 | Classe 3 |
| --- | --- | --- | --- | --- | --- |
| **LBP** | Random Forest | 0,80 | 0,55 | 0,55 | 0,99 |
| **LBP** | SVM | 0,83 | 0,58 | 0,56 | 1,00 |
| **LBP** | RNA (MLP) | 0,85 | 0,60 | 0,53 | 0,99 |
| **VGG16** | **RNA (Winner T8)** | 0,95 | 0,56 | 0,70 | 0,97 |

#### 2. Redes Neurais Convolucionais (Deep Learning)

Nesta abordagem, a rede aprende as características diretamente dos pixels (redimensionados para **224x224**) .

| Arquitetura | Augmentation | Classe 0 | Classe 1 | Classe 2 | Classe 3 |
| --- | --- | --- | --- | --- | --- |
| VGG16 | Sem Aug. | 0,64 | 0,49 | 0,73 | 0,98 |
| VGG16 | Com Aug. | 0,83 | 0,39 | 0,65 | 0,97 |
| ResNet50 | Sem Aug. | 0,93 | 0,72 | 0,90 | 0,99 |
| **ResNet50** | **Com Aug. (Geral)** | 0,94 | 0,83 | 0,89 | 0,99 |

---

### 🔍 Observações Técnicas Importantes

* **Variação de Textura:** A superioridade da **ResNet50 com Augmentation** (Acurácia **91,37%**) demonstra que o uso de rotação e espelhamento foi vital para que o modelo aprendesse a invariância espacial das células cancerígenas .


* **Desafio das Classes 1 e 2:** Todos os modelos apresentaram maior dificuldade nestas classes, sugerindo uma sobreposição visual (similaridade morfológica) que exige extratores mais profundos ou um dataset de treino mais equilibrado.


* **Transfer Learning:** O uso de pesos da *ImageNet* permitiu que os modelos atingissem alta performance mesmo com um número reduzido de épocas ( épocas).

---
