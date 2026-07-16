---
layout: post
title:  "Neural Shop: redes neurais aplicadas a recomendações de produtos"
date:   2026-07-15 10:00:00 -0300
categories: inteligencia-artificial redes-neurais
tags: featured
image: /assets/article_images/neural-shop/neural-shop-recomendacoes.png
---

Como uma aplicação pode aprender as preferências de uma pessoa a partir do seu histórico de compras e transformar esse aprendizado em recomendações?

Foi essa pergunta que guiou o **Neural Shop**, projeto acadêmico que desenvolvi na disciplina de **Fundamentos de IA e LLMs**, da Pós-graduação em Engenharia de Software com IA Aplicada, ministrada por **Erick Wendel**.

![Interface do Neural Shop com perfil do usuário e produtos recomendados](/assets/article_images/neural-shop/neural-shop-recomendacoes.png)

## O projeto

O Neural Shop é um laboratório didático de recomendação que simula um pequeno e-commerce. Na interface, é possível selecionar um cliente, consultar e alterar seu histórico de compras, treinar o modelo e visualizar os produtos classificados pelo percentual de compatibilidade.

Mais do que exibir uma lista de itens, a proposta foi construir e compreender o pipeline completo de uma recomendação:

1. representar produtos e clientes como vetores numéricos;
2. aprender padrões presentes no histórico de compras;
3. recuperar produtos semelhantes;
4. atribuir uma pontuação a cada par cliente-produto;
5. ordenar e apresentar as melhores recomendações ainda não compradas.

## Transformando preferências em números

Redes neurais trabalham com números. Por isso, características como **preço**, **idade**, **categoria** e **cor** precisam ser convertidas em vetores.

Preço e idade são normalizados para o intervalo entre 0 e 1. Categoria e cor utilizam codificação *one-hot*, em que cada possibilidade ocupa uma posição no vetor. Para fins didáticos, as características recebem pesos diferentes: preço (0,4), categoria (0,3), idade (0,2) e cor (0,1).

O vetor do cliente é calculado a partir da média dos vetores dos produtos que ele comprou. Para um perfil sem histórico, apenas a idade contribui inicialmente, permitindo observar na prática o conhecido problema de *cold start*.

## A rede neural no navegador

O modelo foi implementado com **TensorFlow.js** e treinado dentro de um **Web Worker**, mantendo o processamento pesado fora da thread principal da interface.

A rede é sequencial e possui camadas densas de 128, 64 e 32 neurônios, todas com ativação ReLU. A saída usa um único neurônio com ativação sigmoid, produzindo uma pontuação entre 0 e 1 para indicar a compatibilidade entre usuário e produto.

No treinamento, utilizei o otimizador Adam, função de perda `binaryCrossentropy`, lotes de 32 exemplos e 100 épocas. O TensorFlow Visor permite acompanhar a evolução do modelo em tempo real.

As curvas ajudam a tornar o aprendizado visível: a precisão se aproxima de 1 enquanto o erro cai ao longo das épocas. Como o conjunto de dados é pequeno e didático, esses números descrevem o ajuste aos dados de treinamento e não devem ser interpretados, isoladamente, como garantia de desempenho em produção.

## Uma arquitetura híbrida

O projeto evoluiu do exemplo executado apenas no navegador para uma arquitetura que também utiliza **Node.js**, **Express** e **MongoDB**.

Produtos, clientes, compras e vetores são persistidos no banco. Na recomendação, o MongoDB Vector Search recupera até 50 candidatos próximos do vetor do cliente, e a rede neural faz o *reranking*. Por fim, itens já comprados são removidos e os 12 melhores resultados aparecem no catálogo.

Em resumo, o fluxo ficou assim:

```text
Perfil e histórico de compras
        ↓
Vetorização das características
        ↓
MongoDB Vector Search — recuperação de candidatos
        ↓
TensorFlow.js — pontuação e reordenação
        ↓
12 recomendações personalizadas
```

Quando o Vector Search não está disponível em uma instalação local, a aplicação utiliza uma busca exata por similaridade de cosseno, suficiente para o pequeno catálogo acadêmico.

## Principais aprendizados

Este trabalho permitiu conectar conceitos que muitas vezes aparecem separadamente na teoria:

- preparação e normalização de dados;
- codificação de atributos categóricos;
- construção e treinamento de uma rede neural;
- interpretação de `loss` e `accuracy`;
- embeddings e similaridade vetorial;
- recuperação e reordenação de candidatos;
- integração entre frontend, API e banco de dados;
- limites entre uma demonstração acadêmica e um sistema de produção.

Também ficou evidente que um bom sistema de recomendação não depende apenas do modelo. Qualidade dos dados, estratégia de avaliação, tratamento de novos usuários, versionamento de modelos e métricas de negócio são igualmente importantes.

## Conclusão

O Neural Shop transformou conceitos fundamentais de redes neurais em uma experiência visual e interativa. Ao selecionar um perfil, treinar o modelo, acompanhar as curvas e gerar recomendações, é possível observar cada etapa do aprendizado de máquina funcionando dentro de uma aplicação JavaScript completa.

O resultado é um projeto acadêmico, não uma solução pronta para produção, mas representa uma base concreta para continuar explorando recomendadores, bancos vetoriais e aplicações de IA na engenharia de software.

O código-fonte e a documentação estão disponíveis no [repositório da Pós-graduação em Engenharia de Software com IA Aplicada](https://github.com/robsonfagundes/engenharia-de-software-com-ia-aplicada-).
