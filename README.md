## Classificação geoquímica de rochas geradoras de petróleo a partir de dados hiperespectrais e Machine Learning

**INFORMAÇÕES SOBRE O PROBLEMA:**

A espectroscopia consiste em medir as propriedades ópticas de um material, como refletância, para diferentes comprimentos de onda da luz. Para substâncias que são misturas de vários componentes, o espectro de refletância é a superposição de todos os espectros de componentes, portanto, quanto mais abundante um componente, mais fortes são as suas bandas de absorção no espectro da mistura. Neste sentido, a espectroscopia de refletância tem sido explorada extensivamente para coletar informações composicionais de minerais e rochas de maneira não destrutiva e reproduzível.

Um exemplo deste tipo de dado hiperespectral de amostras de rochas pode ser observado abaixo, demonstrando que diferentes amostras, com características mineralógicas e geoquímicas diferentes também apresentam a sua assinatura espectral diferente.

<img src="https://github.com/tainathomg/projeto_final/blob/master/Images/Espectro.png" height="350" width="500">

Análises geoquímicas em amostras de rochas são geralmente caras, demoradas e destrutivas, embora sejam extremamente importantes quando se trata de rochas potencialmente geradoras de petróleo.

Tendo em vista que algumas feições espectrais estão relacionadas a características geoquímicas da rocha, este algoritmo pretende realizar uma classificação geoquímica/orgânica de amostras a partir de feições específicas extraídas de sua resposta espectral. 

Desta forma, a partir de dados geoquímicos coletados de um conjunto de amostras de rochas geradoras, foram determinadas 3 classes quanto à sua fração orgânica (diretamente relacionada ao seu potencial gerador): classe 0 (alta), 1 (média) e 2 (baixa). Também foram selecionadas 4 feições espectrais apontadas na literatura como importantes para esse tipo de rocha e relacionadas à sua fração orgânica para serem consideradas features na classificação: f1, f2, f3 e f4.

Após vários testes com o conjunto de dados, realizado em Python, o algoritmo que melhor se adaptou a este problema foi *Support Vector Machine - SVM*. Sendo assim, um modelo de classificação foi gerado e exportado para que pudesse ser utilizado por outras pessoas para com o mesmo fim. 

Este foi um dos objetivos do projeto final da disciplina de Técnicas de Programação (aplicação do algoritmo gerado), além de criar um ambiente de desenvolvimento para que essa pesquisa possa ser reproduzida por outros pesquisadores, com novos datasets. 

<hr>

**SOLUÇÃO BASEADA EM MICROSSERVIÇOS - DOCKER:**

A solução apresentada conta com 3 microsserviços, conectados de acordo com a figura abaixo:

<img src="https://github.com/tainathomg/projeto_final/blob/master/Images/Esquema_docker.jpg" height="300" width="400">

É importante destacar que nesta solução o próprio usuário do serviço insere o arquivo que será considerado como input e posteriormente extrai os resultados gerados na pasta de trabalho respectiva dentro da pasta "app". 

A imagem abaixo apresenta o arquivo docker-compose desta solução:

<img src="https://github.com/tainathomg/projeto_final/blob/master/Images/docker-compose.JPG" height="500" width="300">

- O container web (nginx) tem como função apresentar ao usuário que pretende fazer a classificação dos dados as duas opções (botões) propostas por esta solução: predizer classes de seus dados hiperespectrais ou então gerar novo modelo de classificação com seu conjunto de dados. A imagem apresenta a parte referente a estes botões no arquivo index.html:

<img src="https://github.com/tainathomg/projeto_final/blob/master/Images/botao_html.JPG" height="300" width="800">

- O container predict tem a função de classificar o dado apresentado pelo usuário em formato de tabela .csv segundo o modelo já treinado previamente (já mencionado) e fornecer como resultado as classes determinadas.

- O container classification tem a função de gerar um novo modelo de classificação, também considerando o algoritmo SVM, para um novo conjunto de dados fornecido pelo usuário. Ele fará todo o processo de dividir o conjunto de dados entre treino e teste, rodar o treino, fazer a validação com o conjunto de teste e depois fornecer como resultados os valores das classes do conjunto de teste, a matriz de confusão desta validação e as métricas de desempenho deste processo.

<hr>

**INSTRUÇÕES PARA FUNCIONAMENTO:**

**1.** Faça o download de todos os arquivos deste projeto e insira em um diretório de interesse;

**2.** Certifique-se de que as portas alocadas para esta aplicação não estejam sendo utilizadas por outros serviços, são elas: 8080, 8081 e 8082.

**3.** Em um terminal Shell, dentro do diretório criado para este projeto, digite:
```
docker-compose up –d
```

**4.** Caso queira acompanhar os logs de serviço, digite:
```
docker-compose logs -f -t
```

**5.** Abra o navegador e digite:
[localhost:8082](http://localhost:8082)

**6.** No navegador estarão contidas as seguintes informações:

<img src="https://github.com/tainathomg/projeto_final/blob/master/Images/html.JPG" height="500" width="600">

**7.** Siga as instruções apontadas no navegador/html sobre os requisitos dos arquivos .csv que serão considerados como entrada para os códigos python de cada container;

**8.** Ao clicar em um dos botões, certifique-se que já se passaram em média 2 minutos desde que a aplicação foi "levantada", pois este é o tempo aproximado para que todo o serviço esteja pronto para rodar;

**9.** Se o usuário realizar todo o procedimento corretamente como solicitado, após clicar um dos botões deve ser encaminhado para o endereço específico de cada aplicação com suas respectivas mensagens de retorno:

*Predição - localhost:8081*

Mensagem: “A previsão dos seus dados foi processada! O arquivo referente ao resultado da classificação encontra-se na pasta de trabalho “app/output” como output_predict.csv.”

*Classificação – localhost:8080*

Mensagem: “A classificação dos seus dados foi processada e um modelo foi gerado! Os arquivos referentes aos resultados da classificação, matriz de confusão e métricas encontram-se na pasta de trabalho "app/output".”
