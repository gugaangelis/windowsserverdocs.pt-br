---
title: Estudo de caso SDK do Windows Admin Center - Thomas Krenn
description: Estudo de caso SDK do Windows Admin Center - Thomas Krenn
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/24/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93b8a450aa86a454ec6febd349fcaa35df590266
ms.sourcegitcommit: 3be280c8638214857dc355b201eb56a04499a5e5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67396756"
---
# <a name="thomas-krennag-extension"></a>Extensão de Thomas Krenn.AG

## <a name="intuitive-server-and-storage-health-management"></a>Gerenciamento intuitivo de integridade de servidor e armazenamento

A extensão de Thomas Krenn.AG Windows Admin Center foi desenvolvida especificamente para 2 nós altamente disponível, [clusters do S2D Micro](https://www.thomas-krenn.com/en/products/application/software-defined-storage/s2d-micro-cluster.html) appliance. A interface da web fácil de usar, gráfica visualiza o status de integridade de um Microdo Cluster por meio de um painel simples e lhe permite fazer uma busca detalhada em dispositivos de armazenamento, interfaces de rede ou todo o cluster para exibir mais detalhes.

A extensão fornece acesso intuitivo a informações normalmente necessárias para as chamadas de suporte e atendimento ao primeiro nível, como números de série, versões de software, utilização de armazenamento e muito mais. Ele é projetado para ser útil para administradores que não tem experiência anterior com a infraestrutura hiperconvergente do Windows Server.

Algumas das informações disponíveis são:
- Informações gerais sobre os nós de Micro e Micro-Cluster
- Sistema operacional / status do dispositivo de inicialização
- Capacidade de HDD e armazenamento em cache o status do SSD
- Eventos de cluster
- Status da rede e informações

Use o painel para determinar o status de integridade e informações importantes do sistema, como números de série, modelo, versão do sistema operacional e utilização do cluster. Além disso, o ventilador, NIC e a integridade geral de hardware do nó são exibidos no painel também.

![Extensão de Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-1.png)

Você pode fazer drill down em dispositivos de armazenamento para exibir números de série, status inteligente e utilização da capacidade. Dispositivos de inicialização também mostram o desgaste de indicadores, realocada setores e energia na hora, que são os melhores indicadores de integridade SSD.

![Extensão de Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-2.png)

O ícone de status do cluster se expande para mostrar um resumo dos detalhes de operacional do cluster.

![Extensão de Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-3.png)

Depois que a testemunha de nuvem do Azure neste Microdo Cluster não estava disponível em toda noite, uma rápida análise é o suficiente para identificar o problema. Clicar em "Notificações" imediatamente lista eventos relevantes de correção rápida. Eventos de cluster estão localizados e determinados pelo idioma do sistema operacional base. A extensão em si dá suporte a inglês e alemão.

![Extensão de Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-4.png)

Informações de rede também estão prontamente disponíveis.

![Extensão de Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-5.png)

Com base nos comentários dos clientes, também implementamos "Dark modo" disponível no Windows Admin Center v1904. Isso é reconfortante em datacenters escuro em gabinetes mal iluminados e armários. Ele também torna Windows Admin Center mais acessível, reduzindo a indicação de espera para que os administradores com determinados deficiências visuais.

![Extensão de Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-6.png)

Thomas Krenn percebidos imediatamente que a usabilidade e a acessibilidade para que os administradores não treinados seria a chave para uma experiência de cliente excelente para infraestrutura hiperconvergida no mercado de negócios de pequeno e médio porte. Extensão de Cluster Micro do Thomas Krenn perfeitamente complementa os recursos de gerenciamento do Windows Admin Center nativos HCI incluindo informações de hardware proprietário no painel e agrupando novamente as informações de integridade do cluster importante em uma nova interface amigável a humanos.

Durante o processo de desenvolvimento decidiu implantar Windows Admin Center 1904 em uma configuração de alta disponibilidade no cluster em si, garantindo a capacidade de gerenciamento, mesmo após falhas de nó. A extensão vem pré-instalado, assim como o todo o sistema operacional.

A extensão foi criada em paralelo com o Windows Admin Center 1904 sendo desenvolvidos na Microsoft. Cooperação e problemas de comentários contínuos exposto em ambos os lados que foram resolvidos em conjunto antes do produto iniciado com êxito em abril de 2019. Thomas Krenn é incrivelmente orgulho de ser um dos primeiros totalmente dar suporte e implementar novos recursos do Windows Admin Center 1904.
