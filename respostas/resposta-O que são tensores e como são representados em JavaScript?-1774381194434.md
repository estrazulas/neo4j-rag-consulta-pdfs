Tensores emTensorFlow.js são estruturas de dados fundamentais que representam vetores, matrizes ou arrays multi-dimensionais contendo **apenas valores numéricos**, utilizados pelos algoritmos de machine learning para identificar padrões em dados. Em JavaScript, eles são representados como arrays padrão (ou arrays aninhados para dimensões superiores) de números, já que a biblioteca não processa diretamente objetos JavaScript misturados (como strings, objetos ou outros tipos não numéricos).

Para ilustrar com o exemplo do contexto fornecido:  
Imagine que queremos usar a propriedade "cor" (azul, vermelho, verde) das pessoas para treinar um modelo. Como o TensorFlow.js só entende números, convertemos essa informação categórica em uma representação numérica **one-hot encoding**, onde cada cor ativa recebe o valor `1` e as demais `0`:

- Erick (azul): `[1, 0, 0]`  
- Ana (vermelho): `[0, 1, 0]`  
- Carlos (verde): `[0, 0, 1]`  

Esses vetores podem ser organizados em uma **matriz 2D** (tensor de ordem 2), representada em JavaScript como um array de arrays:  
```javascript
[
  [1, 0, 0],  // Erick
  [0, 1, 0],  // Ana  [0, 0, 1]   // Carlos
]
```

Esta estrutura é exatamente o que chamamos de **tensor** no TensorFlow.js: um contêiner numérico puro que permite que os algoritmos realizem operações matemáticas eficientes (como multiplicações de matrizes) para aprender com os dados. É importante lembrar que, antes de usar tais dados no modelo, é necessário converter qualquer informação não numérica (como nomes, cores ou localizações) para esse formato, tal como demonstrado no exemplo do contexto. Assim, os tensores servem como a "linguagem universal" entre seus dados JavaScript e os algoritmos de aprendizado de máquina! 😊