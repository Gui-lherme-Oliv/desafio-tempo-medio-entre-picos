# Solução para o Desafio de programação - Tempo Médio Entre Picos
#### Autor: Guilherme Oliveira da Rocha Cunha
### 1. Ideias e estratégias
Minha estratégia foi a seguinte:
1. Dividi a lista com os valores referentes à leitura do sensor em sublistas que representam os intervalos que possuem picos, ou seja, intervalos onde o primeiro e último valores sejam maiores ou iguais a 50.
2. Encontrei o maior valor dentro de cada sublista e defini como sendo o pico verdadeiro.
3. Criei uma lista contendo os índices dos picos verdadeiros, índices esses que representam o tempo em segundos de cada valor.
4. Com a lista contendo os tempos em segundos, encontrei o tempo médio entre os picos e apresentei no formato mm:ss.

Por exemplo, em uma lista = [14, 25, 53, 89, 75, 74, 37, 91, 12, 64, 51] eu teria 3 sublistas, [53, 89, 75, 74], [91] e [64, 51]. Encontrando o maior valor em cada sublista eu encontro os picos verdadeiros da lista, são eles 89, 91 e 64. Nesse caso, a lista contendo os tempos dos picos verdadeiros ficaria [3, 7, 9], e o tempo médio entre os picos seria 00:03.

Implementei algumas funções para chegar ao resultado:

#### 1.1. Função ``intervalos``
1. A função recebe como parâmetro a ``lista`` contendo os valores referentes à leitura do sensor.
2. Cria uma lista vazia chamada ``intervalos`` que será uma lista de tuplas usada para armazenar os intervalos encontrados na lista de entrada e define que o início do intervalo terá valor *None*.
3. Itera sobre a lista de entrada, obtendo o índice (``i``) e o valor (``valor``) de cada elemento.
4. Se o valor atual da iteração for maior ou igual a 50 e o início do intervalo ainda não foi definido, o início do intervalo será definido como o índice do valor atual (``i``).
5. Se o valor atual da iteração não é maior ou igual a 50, mas já estamos dentro de um intervalo, então adiciona à sublista ``intervalos`` uma tupla que contém o início do intervalo e o índice do valor anterior ao valor atual da iteração (``i - 1``). Então, o início do intervalo volta a ser *None*, indicando que o intervalo atual foi concluído.
6. Caso ainda exista um intervalo pendente (o loop terminou, mas ainda não se adicionou o último intervalo), então adiciona à sublista ``intervalos`` uma tupla que representa o último intervalo pendente.
7. Retorna ``intervalos``

#### 1.2. Função ``imprimir_picos_verdadeiros``
1. A função recebe como parâmetros a ``lista`` contendo os valores referentes à leitura do sensor e a sublista ``intervalos`` contendo todos os intervalos.
2. Itera sobre a sublista ``intervalos`` e obtém o índice (``i``) e o valor (tupla) de cada elemento.
3. Como o primeiro e último valor de cada tupla em ``intervalos`` representam índices em ``lista``, utilizo o slicing para obter o intervalo com os valores. Desse intervalo, obtenho o maior valor e armazeno na variável ``maximo``. Essa variável representa os picos verdadeiros.
4. Imprime todos os picos verdadeiros.

#### 1.3. Função ``tempo_medio_picos``
1. A função recebe como parâmetros a ``lista`` contendo os valores referentes à leitura do sensor e a sublista ``intervalos`` contendo todos os intervalos. Por conta dessa função foi necessário importar a biblioteca *datetime*.
2. Inicializa uma lista vazia chamada ``tempos``, onde serão armazenados os índices dos picos verdadeiros encontrados.
3. Cria uma sublista da lista original ``lista`` utilizando o intervalo fornecido, criando-se uma sublista com os valores do intervalo. O intervalo é inclusivo.
4. Encontra o valor máximo da sublista. Depois soma o índice relativo à lista original, do primeiro elemento da sublista; com o índice relativo à sublista do valor máximo da sublista, assim encontrando o índice relativo à lista original do valor máximo do intervalo. Fiz isso para garantir que caso o mesmo número apareça mais de uma vez na lista original mas em posições diferentes, então seus índices também serão diferentes, logo não haverão repetições de índice.
5. Os índices dos valores máximos são salvos na lista ``tempos``.
6.  Utiliza uma list comprehension para percorrer os elementos de ``tempos`` e calcular a diferença entre cada par de valores consecutivos, armazenando essas diferenças em uma lista chamada ``diferencas``. Em seguida calcula a média dessas diferenças, ou seja, a soma dividida pelo tamanho da lista ``diferencas`` (esse tamanho da lista representa a quantidade de picos) e salva a média na variável ``media_segundos``, pois o tempo é dado em segundos.
7.  Converte ``media_segundos`` em um objeto *timedelta* (``td``) e depois converte esse objeto em uma string no formato de h:m:s (horas:minutos:segundos).
8.  Se o valor em horas for zero, então retorna apenas os minutos e segundos como uma string formatada. Se não for, retorna uma string contendo horas, minutos e segundos.

#### 1.4. Função ``ler_dados``
Essa função recebe como parâmetro o arquivo com os valores referentes à leitura do sensor. Ela lê todas as linhas do arquivo aberto e armazena em uma lista chamada ``linhas``, onde cada elemento da lista corresponde a uma linha do arquivo. Para cada linha, remove quaisquer espaços em branco no início ou no final (usando strip()) e converte o resultado para um inteiro. Por último, retorna a lista.

### 2. Geração de exemplos de entrada
Considerei como entrada arquivos txt que seriam inseridos pelo usuário, contendo os valores referentes à leitura do sensor. Considero que os valores nesse arquivo já estão ordenados, ou seja, cada valor de leitura está no tempo correto. O valor na primeira linha está no tempo inicial (00 segundos), o valor na segunda linha está no tempo seguinte (01 segundos), o valor na terceira linha está no tempo seguinte (02 segundos), e assim por diante.

Para isso implementei um código simples chamado **gerador_dados** para a criação de arquivos com números inteiros aleatórios variando de 0 a 100. O arquivo *exemplo.txt* é o arquivo disponibilizado no repositório do desafio; o arquivo *exemplo2.txt* contém 50 números aleatórios; *exemplo3.txt* contém 100 números aleatórios; e *exemplo4.txt* contém 200 números aleatórios.

### 3. Dificuldades e dúvidas
Inicialmente a dificuldade foi encontrar a lógica para a resolução do problema. Após isso a maior dificuldade foi como implementar, como "tirar a ideia do papel" e transformar em código. 

A maior dúvida é referente à validação dos resultados. Para entradas com até cerca de 100 valores, uma abordagem empírica, analisando cada um dos valores da lista para descobrir se é um pico verdadeiro é fácil, e um recurso visual como um gráfico de linha também serve para validar os resultados. A partir de 200 ou 300 valores, fica difícil de distinguir os valores no gráfico e um pouco mais complicado de verificar valor por valor na lista. A medida que aumenta a quantidade de dados, fica mais complicado continuar nessa abordagem. Então a maior dúvida seria como validar a eficácia do programa para um grande volume de dados.

### 4. Execução da solução
Minha solução foi implementada utilizando a linguagem Python. Não precisei instalar nenhum compilador/interpretador por utilizei o Google Colab ([link](https://colab.research.google.com/)) que é um serviço de armazenamento em nuvem de notebooks voltados à criação e execução de códigos em Python, diretamente em um navegador, sem a necessidade de nenhum tipo de instalação de software em uma máquina.

</div>
