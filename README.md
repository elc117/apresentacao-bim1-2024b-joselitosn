# apresentacao-bim1-2024b-joselitosn
> **Disciplina:** ELC117 - Paradigmas de Programação - 2024b  
> **Aluno:** Vinicios Ragagnin Portella  
> **Apresentação:** Função 10

## Para começar, um contexto...
A visão computacional é um campo da inteligência artificial que utiliza algoritmos de aprendizagem de máquina para interpretar e entender o conteúdo de imagens e vídeos. Uma das tarefas fundamentais na visão computacional é a detecção de objetos, que envolve localizar e identificar objetos de interesse dentro de uma imagem ou um vídeo.

Durante o processo de detecção de objetos, três informações principais são geradas:

- Bounding Boxes (BBoxes): São retângulos ou caixas delimitadoras que representam a localização de objetos detectados em uma imagem. Cada bounding box é definida por quatro coordenadas (x1,y1,x2,y2), que correspondem aos cantos superior esquerdo e inferior direito da caixa. As bounding boxes ajudam a indicar onde os objetos estão localizados na imagem.
- Scores: Para cada bounding box, um score é atribuído, representando a confiança do modelo na detecção do objeto. Esse score é um valor entre 0 e 1, onde valores mais altos indicam maior confiança de que um objeto foi detectado corretamente. O score é fundamental para filtrar detecções irrelevantes ou errôneas.
- Classes: Cada objeto detectado é classificado em uma de várias categorias (ou classes) pré-definidas. Por exemplo, em um modelo treinado para detectar animais pode incluir "gato" e "cachorro".

A imagem abaixo ilustra as bounding boxes, classes e scores.

[![Bounding boxes classification of a dog and a cat](http://d2l.ai/_images/output_anchor_f592d1_192_0.svg 'Codey the Codecademy mascot')](http://d2l.ai/chapter_computer-vision/anchor.html)

## Dados para teste
Em Haskell, podemos usar listas para representar bounding boxes, scores e classes de objetos. Essas listas têm o mesmo número de elementos, sendo que o primeiro elemento em cada uma delas refere-se ao primeiro objeto, e assim por diante. Por exemplo, no código abaixo, o primeiro objeto é da classe 0, com score 0.95 e com bounding box (34.0, 60.0, 200.0, 320.0).

```
-- Bounding boxes: (xmin, ymin, xmax, ymax)
boundingBoxes :: [(Float, Float, Float, Float)]
boundingBoxes = [ (34.0, 60.0, 200.0, 320.0),
              	(100.0, 150.0, 250.0, 380.0),
              	(300.0, 220.0, 450.0, 450.0) ]

-- Scores: Confidence scores for each detection
scores :: [Float]
scores = [0.95, 0.80, 0.60]

-- Classes: Class 0 or 1 representing two object types
classes :: [Int]
classes = [0, 1, 0]
```

## Agora sim, as funções...
### Função 10
Resolva o exercício 9 usando lambda.

### Função 09
Crie uma função que receba uma lista de bounding boxes e calcule o somatório das áreas dessas bounding boxes.  
Resolva esta função sem usar lambda.

Exemplo de uso:
```
ghci> sumAreasOfBoundingBoxes boundingBoxes
112160.0
```

Nome e tipo da função:
```
sumAreasOfBoundingBoxes :: [(Float, Float, Float, Float)] -> Float
```
## Tentativas
Em primeira análise, decidi desmontar a função em três partes:
- Somatório da lista de àreas;
- Aplicação do cálculo da área para cada caixa delimitadora;
- Cálculo da área do retângulo.

### Área do retângulo
$$\begin{equation}A = b \times h \text{, onde } b \text{ é a base e } h \text{ é a altura.}\end{equation}$$

É importante notar que no contexto do problema recebemos o nome das variáveis xmin, ymin, xmax, ymax, o que me levou a crer que os dois primeiros valores referem-se ao ponto mais baixo e para a esquerda, e os últimos dois valores referem-se ao ponto mais alto e para a direita.  
Caso contrário, seria necessário utilizar função que obtenha o módulo do cálculo que indica o comprimento.

$$\begin{equation}b = |x_{\text{max}} - x_{\text{min}}| \quad \text{e} \quad h = |y_{\text{max}} - y_{\text{min}}|\end{equation}$$  

Por fim, obtemos a função lambda que recebe a tupla com os pontos delimitadores e calcula-se a àrea.
```
\(xmin, ymin, xmax, ymax) -> (xmax - xmin) * (ymax - ymin)
```

### Aplicação da função lambda para cada caixa delimitadora
Utilizou-se a função `map`, que recebe dois parametros uma função e uma lista. Desta forma a função será aplicada para cada membro da lista e o resultado inserido em uma nova lista.
```
map (\(xmin, ymin, xmax, ymax) -> (xmax - xmin) * (ymax - ymin)) boundingBoxes
```

### Somatório das àreas
Nesta última parte, recebemos a lista com as àreas e realizamos o somatório através da função `sum` que recebe como argumento uma lista e retorna a soma de todos os valores contidos na mesma.
```
sum (map (\(xmin, ymin, xmax, ymax) -> (xmax - xmin) * (ymax - ymin) boundingBoxes)
```

## Resultado
Juntando as partes temos o código final abaixo:
```
-- Bounding boxes: (xmin, ymin, xmax, ymax)
boundingBoxes :: [(Float, Float, Float, Float)]
boundingBoxes = [ (34.0, 60.0, 200.0, 320.0),
                  (100.0, 150.0, 250.0, 380.0),
                  (300.0, 220.0, 450.0, 450.0) ]

sumAreasOfBoundingBoxes :: [(Float, Float, Float, Float)] -> Float
sumAreasOfBoundingBoxes boundingBoxes = sum (map (\(xmin, ymin, xmax, ymax) -> (xmax - xmin) * (ymax - ymin)) boundingBoxes)

main :: IO ()
main = do
    print $ sumAreasOfBoundingBoxes boundingBoxes
```

## Tentativas falhas
Durante a definição da função lambda, tentei utilizar o `where` para definir uma função para calcular o comprimento das laterais, mas não foi possível pois a função lambda cria um novo escopo que está fora da função principal e não é possível acessar suas variáveis através de _pattern matching_.

## Fontes
[https://pt.wikibooks.org/wiki/Haskell/Lambdas_e_operadores]  
[https://wiki.haskell.org/Lambda_abstraction]  
[https://onecompiler.com/haskell/]  
[https://www.codecademy.com/resources/docs/markdown/images]  
[https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions]  
[https://liascript.github.io/course/?https://raw.githubusercontent.com/AndreaInfUFSM/elc117-2024b/main/classes/07/README.md#9]  
[https://wiki.haskell.org/Let_vs._Where]  
