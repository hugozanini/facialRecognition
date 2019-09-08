# Facial Recognition


Este repositório implementa um método de reconhecimento facial em tempo real utilizando [OpenCV](https://opencv.org/) e a rede neural [Facenet](https://arxiv.org/pdf/1503.03832.pdf) .




# Requeriments
O projeto está implementado em python e é necessário a instalação dos seguintes pacotes para a execução dos códigos:

- imutils==0.5.3
- joblib==0.13.2
- numpy==1.17.2
- opencv-contrib-python==4.1.1.26
- scikit-learn==0.19.2
- scipy==1.3.1

*É recomendável a criação de um ambiente virtual antes da instalação* 

## Estrutura do projeto 

O projeto está organizado da seguinte maneira:
 ![enter image description here](https://lh4.googleusercontent.com/F9RIVVpcwXImOfvKoBCRKAbHeTnEYmsA5QbxzLjVKsMkHxpayVSuYjvKzvg4qNFdP7oaW1SMhNjCt6PxFRZ2ht1AcBvci43b3LygjdChkGlSlEO3Wbx-N0q3fm1NH8V1iuNEgoTQ)

A seguir, são descritos brevemente os arquivos e diretórios presentes no projeto:
-  **dataset**: Contém as imagens de rostos utilizadas no treino organizadas em subfolders.  
-   **face_detection_model**: contém um modelo de deep learning Caffe pré-treinado fornecido pelo OpenCV para detectar rostos. Este modelo detecta e localiza rostos em uma imagem.
-   **output**: contém as saídas do reconhecimento armazenadas em arquivos pickle.
-   **extract_embedding**.py: utiliza extrator de features para gerar um vetor 128-D que descreve uma face. Todas as faces precisam passar por este processo.
-   **openface_nn4.small2.v1.t7**: Um modelo de deep learning em Torch que produz os embedding faciais em 128-D.
-   **train_model**.py: Treina o modelo.
- **recognize_video.py**: Reconhece as faces no video da webcam.


## Tests

Para fim de testes, será utilizado um modelo pré-treinado da FaceNet com diferentes faces. O reconhecimento facial será feito em tempo real, utilizando a webcam do computador em que os códigos estão sendo executados como entrada de vídeo.

Acredita-se que dessa forma o projeto irá conseguir simular um cenário real em que robustez e eficiência são fundamentais.

### Dataset

O dataset utilizado no projeto contém 6 fotos diferentes de três pessoas:

-   Bill Gates
    
-   Steve Jobs
  
-   Unknown: representa pessoas desconhecidas e que já foram vistas pelo método. As imagens foram retiradas do site [shutterstock](https://www.shutterstock.com/?pl=PPC_GOO_BR_BD-264663191462&cr=ec&kw=shutterstocks&gclid=CjwKCAjwzdLrBRBiEiwAEHrAYiGZ2z7Orb958rPQUfwsDix8a5cnI-iVuTu8McXYfxPLgTjWeLPIcxoC52MQAvD_BwE&gclsrc=aw.ds).
    
Caso o usuário deseje incluir sua própria foto, basta acessar a pasta de datasets e adicionar uma nova pasta com o seu nome e algumas fotos (recomenda-se entre 10 e 20).
### Executando os códigos

O primeiro passo é a execução do extrator de features:

    python extract_embeddings.py --dataset dataset \  
    --embeddings output/embeddings.pickle \  
    --detector face_detection_model \  
    --embedding-model openface_nn4.small2.v1.t7

Posteriormente, o modelo precisa ser treinado:

    python train_model.py --embeddings output/embeddings.pickle \  
    --recognizer output/recognizer.pickle \  
    --le output/le.pickle

O reconhecimento das faces em tempo real é feito a partir do código a seguir:

    python recognize_video.py --detector face_detection_model \  
    --embedding-model openface_nn4.small2.v1.t7 \  
    --recognizer output/recognizer.pickle \  
    --le output/le.pickle

Visando identificar a robustez do método a variações externas como intensidade de luz e qualidade da imagem, utilizando um smartphone, colocou-se em frente a webcam uma foto de Bill Gates e Steve Jobs. A abaixo mostra os resultados.

![Resultado da identificação facial](https://lh3.googleusercontent.com/tWBfre17r1h7jjijtZiHBeTSZPcq99bxsBny4eLQi2iRiPs4iypN5q8nOydi63rn3zcB7oumVapDKA)

Este teste cobre todos os aspectos básicos da identificação facial em imagens pois, identifica múltiplas faces em uma imagem, apresenta bons resultados para condições adversas e com um número reduzido de imagens de treino e executa todo o processo em tempo real a partir de vídeos.
## References
 [Face recognition with OpenCV, Python, and deep learning](https://www.pyimagesearch.com/2018/06/18/face-recognition-with-opencv-python-and-deep-learning/)
