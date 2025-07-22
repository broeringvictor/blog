---
title: "Aprendizado sobre bots - estudo sobre API do sistema, ponteiros de memória e visão computacional, parte 1"
seoTitle: "Bots: API, Ponteiros, Visão Computacional"
seoDescription: "Uma investigação técnica sobre a guerra entre bots de jogos e sistemas anti-cheat. Explore métodos como leitura de memória, análise de rede e em nv. kernel."
datePublished: Tue Jul 22 2025 17:42:05 GMT+0000 (Coordinated Universal Time)
cuid: cmdetnrwe000402l4al2v6zu6
slug: aprendizado-sobre-bots-um-estudo-sobre-api-do-sistema-ponteiros-de-memoria-e-visao-computacional-parte-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1753208275661/a5ae5de4-8a7c-413f-9703-11223fe4f013.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1753205718878/16b9427d-f8ce-42c6-9760-8be8b12ba9d3.png
tags: bot, game-development, net

---

> A partir de uma simples curiosidade sobre bots em jogos, iniciei uma investigação técnica aprofundada. Para compreender a essência do problema, desenvolvi um bot do zero, analisando a guerra de engenharia reversa travada entre desenvolvedores e os modernos sistemas anti-cheat.

Para entender a raiz do problema, decidi construir meu próprio bot do zero. Este projeto me levou a uma jornada fascinante pela corrida armamentista tecnológica travada entre os desenvolvedores de bots e os sistemas anti-cheat.

Este artigo explora os métodos mais sofisticados e "invisíveis" que bots utilizam para operar em larga escala, focando em como eles interagem diretamente com o jogo sem precisar "vê-lo", e como as defesas modernas tentam combatê-los.

## Headless: extraindo dados da memória e da rede

Isso me levou a questionar sobre a escala da operação: como é possível gerenciar centenas de instâncias do jogo simultaneamente, talvez em um ambiente de servidor?

Fiquei intrigado em como eles alcançam isso, provavelmente executando clientes em modo **'headless'** (sem interface gráfica), o que otimiza drasticamente os recursos ao ignorar a renderização visual e extrair dados cruciais — como coordenadas e status — diretamente da **memória do processo** ou dos **pacotes de rede**.

A mecânica por trás disso é fascinante e contorna completamente a necessidade de "ver" o jogo. No método de **leitura de memória**, o bot utiliza funções de baixo nível do sistema operacional (como `ReadProcessMemory` no Windows) para acessar o espaço de memória do jogo.

Com os endereços de memória corretos, conhecidos como ponteiros e offsets, ele pode ler qualquer informação: da vida do personagem às coordenadas de um inimigo.

Já a **análise de pacotes de rede** é ainda mais sofisticada: o bot "escuta" a comunicação entre o cliente e o servidor. Ao decifrar esses pacotes, ele obtém o estado real do jogo e pode forjar suas próprias respostas para executar ações.

Um exemplo desta função era a capacidade do bot de deslogar ao reconhecer que um administrador entrou no mapa, com base no seu Id.

## A escalada da detecção e evasão

Contudo, essa abordagem, embora poderosa, é a primeira a ser combatida pelos anti-cheats modernos. Sistemas de proteção realizam varreduras constantes para detectar processos externos que tentam ler a memória do jogo.

Além disso, eles validam a integridade dos próprios arquivos em memória e utilizam ofuscação de dados e criptografia no tráfego de rede para tornar a vida de quem tenta bisbilhotar extremamente difícil. Isso torna a maioria dos bots baseados nesses métodos rapidamente obsoleta.

É aqui que a batalha desce para as camadas mais baixas do sistema. A possibilidade de um bot se tornar verdadeiramente indetectável reside em executá-lo no mesmo nível de privilégio que os anti-cheats mais agressivos, como o Vanguard.

Se um anti-cheat atua em **nível de kernel (Ring 0)**, como o Vanguard, carregado logo após a BIOS para monitorar o sistema, a única forma de contorná-lo seria com uma ferramenta que também opere nesse nível.

Um bot implementado como um driver de kernel poderia, teoricamente, interceptar dados e manipular o jogo de uma forma que seria invisível para as defesas que rodam em camadas superiores (user-mode).

Essencialmente, a estratégia seria combater fogo com fogo, criando uma ferramenta que se esconde no mesmo "ponto cego" que o protetor utiliza para vigiar.

A questão seguinte foi: como garantir que apenas o cliente oficial possa se comunicar com o servidor? A solução é um tipo de "aperto de mão secreto" e contínuo, conhecido como **atestado de integridade do cliente**.

Funciona assim: o cliente oficial é compilado com uma chave que só ele conhece.

Periodicamente, o servidor pede que o cliente "prove sua identidade" resolvendo um desafio criptográfico que só pode ser solucionado com essa chave. Um cliente falso, sem a chave, falha na verificação e a conta pode ser banida.

O impacto disso na escalabilidade dos bots é direto.

Embora um único bot pudesse, com enorme esforço de engenharia reversa, "roubar" a chave, o verdadeiro desafio é usá-la em escala.

Executar essa lógica criptográfica complexa para centenas de contas multiplicaria o consumo de CPU e memória, tornando a 'bot farm' financeiramente inviável e destruindo o propósito de uma operação leve.

## Conclusão

Esta análise revela que a criação de bots modernos vai muito além de simples scripts de automação. É uma batalha de engenharia reversa, criptografia e privilégios de sistema. A guerra entre a detecção e a evasão é constante, escalando das varreduras de memória até drivers em nível de kernel e desafios criptográficos complexos.

A exploração deste universo é fascinante e complexa.. Em um próximo artigo, abordarei outras técnicas, como a automação via **Visão Computacional** e o uso da **API do Windows** para simular interações humanas, além de um guia prático sobre como um bot simples pode ser construído em .NET.

Se o tema lhe interessa, conecte-se comigo para acompanhar a continuação.