---
layout: post
title:  "Solitaire AI: ensinando o navegador a jogar Paciência"
date:   2026-07-27 10:00:00 -0300
categories: inteligencia-artificial web-machine-learning
tags: featured
image: /assets/article_images/solitaire-ai/solitaire-ai-tabuleiro.png
---

Como fazer uma aplicação observar um jogo no navegador, entender o estado do tabuleiro, escolher uma jogada válida e executá-la automaticamente?

Foi essa pergunta que guiou o **Solitaire AI**, projeto acadêmico que desenvolvi no módulo de **Web Machine Learning**, da Pós-graduação em Engenharia de Software com IA Aplicada, ministrada por **Erick Wendel**.

![Tabuleiro do Solitaire AI antes do início da automação](/assets/article_images/solitaire-ai/solitaire-ai-tabuleiro.png)

## O projeto

O ponto de partida do módulo foi o clássico Duck Hunt. Nesse tipo de jogo, o fluxo de automação é relativamente direto: capturar a tela, detectar um pato, converter a detecção em coordenadas e clicar no alvo.

![Duck Hunt executando a detecção do alvo e exibindo as coordenadas previstas](/assets/article_images/solitaire-ai/duck-hunt-deteccao.png)

Decidi aplicar o mesmo conceito ao Solitaire. O desafio ficou maior porque encontrar uma carta não é suficiente. A aplicação também precisa reconstruir as pilhas, respeitar as regras da Paciência, avaliar os movimentos possíveis e escolher uma ação que faça o jogo avançar.

O ciclo desenvolvido ficou assim:

```text
Captura do tabuleiro
        ↓
Processamento em um Web Worker
        ↓
Reconstrução do estado do jogo
        ↓
Geração e pontuação das jogadas válidas
        ↓
Conversão da ação em coordenadas
        ↓
Clique automático
        ↓
Nova captura para confirmar o resultado
```

## Da interface para uma imagem

O Solitaire é construído com elementos HTML, e não com um único `canvas`. Por isso, utilizei o **html2canvas** para rasterizar o tabuleiro. O resultado é convertido em um `ImageBitmap`, formato que pode ser transferido para outra thread sem a necessidade de copiar todos os pixels na memória.

O bitmap é enviado a um **Web Worker**, que representa a fronteira de inferência da arquitetura. No worker, a imagem é desenhada em um `OffscreenCanvas`, processada e liberada ao final do ciclo.

Esse isolamento mantém a captura e o processamento fora da thread principal da interface, evitando que a automação bloqueie a interação e a renderização do jogo.

## Representando o estado do tabuleiro

Cada carta visível é representada por seu valor, naipe, cor, orientação e uma caixa delimitadora com as coordenadas relativas ao tabuleiro. A partir dessas observações, a aplicação organiza:

- o estoque;
- o descarte;
- as quatro fundações;
- as sete colunas do *tableau*.

Essa representação cria uma separação importante: o solver não precisa conhecer os elementos HTML nem acessar diretamente todas as funções internas do jogo. Ele recebe um estado estruturado, semelhante ao que seria produzido por um detector de objetos.

## Visão computacional e transparência técnica

A arquitetura foi preparada para receber as predições de um modelo como o **YOLO**, mas esta versão acadêmica não treina um detector para reconhecer as 52 cartas.

Enquanto esse modelo não existe, valores, naipes e caixas delimitadoras são obtidos do estado interno do jogo por meio de um fallback determinístico. A captura com `createImageBitmap`, a transferência para o Web Worker, o processamento fora da thread principal, as coordenadas, o solver e os cliques são reais. Somente a classificação visual das cartas é substituída pelo fallback.

Essa decisão permitiu estudar e implementar o pipeline completo em JavaScript sem esconder uma limitação importante ou atribuir ao modelo uma capacidade que ele ainda não possui.

## Um solver baseado em regras e heurísticas

Depois de reconstruir o tabuleiro, o solver gera os movimentos permitidos pelas regras da Paciência. Ele verifica, por exemplo, se uma carta pode iniciar ou avançar uma fundação, se pode ser movida para uma coluna em ordem decrescente e com cores alternadas, e se um rei pode ocupar uma coluna vazia.

As ações possíveis recebem pontuações. Revelar uma carta fechada tem a maior prioridade, seguido por liberar uma coluna, utilizar uma carta do descarte, alimentar uma fundação e organizar sequências no *tableau*.

Quando não existe um movimento de carta disponível, a automação compra uma carta do estoque. Quando o estoque termina, tenta reciclar o descarte. Se a partida deixa de apresentar progresso, uma nova distribuição pode ser iniciada.

O solver também registra uma assinatura do estado combinada com cada ação executada. Esse histórico impede que a aplicação repita indefinidamente o mesmo movimento no mesmo tabuleiro.

## Transformando decisões em cliques

Após escolher uma ação, o controller utiliza a caixa delimitadora da carta para calcular o ponto do clique. A coordenada local é convertida para a posição correspondente no viewport, e um `MouseEvent` executa a interação.

Depois de cada clique, a aplicação aguarda a atualização da interface e compara o estado anterior com o novo. Somente então inicia outra captura. Esse mecanismo de feedback confirma se a ação teve efeito e evita inferências sobrepostas.

## Métricas além da palavra “acurácia”

O projeto registra capturas, inferências, cartas processadas, ações tentadas, ações bem-sucedidas, cliques no estoque, reciclagens, reinícios, vitórias e tempos médios.

A principal métrica é a `actionAccuracy`: a porcentagem de ações que realmente alteraram o estado do tabuleiro. Ela mede a eficácia da automação, e não a precisão de um detector visual.

Como a versão atual utiliza o fallback determinístico, a `modelAccuracy` permanece como não disponível. Uma acurácia de visão computacional só poderia ser calculada corretamente com um modelo treinado e um conjunto independente de imagens rotuladas.

![Solitaire AI em execução com o relatório periódico de métricas no console do navegador](/assets/article_images/solitaire-ai/solitaire-ai-metricas.png)

Na execução acima, o console mostra o detector identificado como `development-fallback`, a acurácia das ações, os tempos de captura e inferência e a evolução da partida enquanto a IA movimenta as cartas.

## Principais aprendizados

Este projeto mostrou, na prática, como diferentes componentes precisam trabalhar juntos para que uma aplicação perceba, decida e aja:

- captura e rasterização de interfaces HTML;
- transferência eficiente de imagens com `ImageBitmap`;
- processamento paralelo com Web Workers e `OffscreenCanvas`;
- representação estruturada de observações visuais;
- conversão entre sistemas de coordenadas;
- modelagem das regras de um domínio;
- uso de heurísticas para escolher ações;
- prevenção de ciclos por assinatura de estados;
- execução e validação de interações automáticas;
- definição de métricas coerentes com o que realmente está sendo medido.

O aprendizado mais importante foi perceber que detectar objetos é apenas uma parte do problema. Em tarefas mais complexas, a aplicação também precisa transformar percepções em estado, aplicar regras, tomar decisões e verificar o efeito de cada ação.

## Limitações e próximos passos

O projeto é uma demonstração acadêmica e possui limitações intencionais. O fallback conhece o estado interno do jogo, a estratégia heurística não pesquisa todas as soluções possíveis e algumas distribuições podem ser insolúveis ou difíceis para as regras atuais.

O próximo passo natural seria treinar um detector para reconhecer valores e naipes diretamente a partir das imagens. Como a fronteira do worker e o formato das predições já estão definidos, esse modelo poderia substituir o fallback sem exigir mudanças no solver ou no executor de ações.

## Conclusão

![Solitaire AI após vencer a partida, com o relatório de vitória no console](/assets/article_images/solitaire-ai/solitaire-ai-vitoria.png)

O Solitaire AI transformou um jogo conhecido em um laboratório de percepção, decisão e automação no navegador. O projeto conecta APIs modernas da Web, processamento paralelo, representação de estado, regras de negócio, heurísticas e métricas em um único ciclo executado inteiramente com JavaScript.

Mais do que ensinar uma máquina a clicar em cartas, o exercício mostrou como decompor um problema de IA em etapas verificáveis e como comunicar com clareza a diferença entre o que já foi implementado e o que ainda depende de um modelo treinado.

O código-fonte e a documentação estão disponíveis no [repositório do projeto](https://github.com/robsonfagundes/engenharia-de-software-com-ia-aplicada-/tree/main/01-fundamentos-de-ia-e-llms/modulo-04/vencendo-qualquer-jogo).
