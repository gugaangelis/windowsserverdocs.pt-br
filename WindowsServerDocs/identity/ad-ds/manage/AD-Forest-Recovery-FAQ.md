---
title: Recuperação de floresta do AD - perguntas Frequentes
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.openlocfilehash: 62f0653d0d35a1fb21d7de452401c48291e4259c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87959054"
---
# <a name="ad-forest-recovery---faq"></a>Recuperação de floresta do AD - perguntas Frequentes

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2, Windows Server 2003

Este documento contém perguntas frequentes (FAQs) sobre a recuperação da floresta:

## <a name="general-recovery"></a>Recuperação geral

**P: o que posso fazer para acelerar a recuperação?**

Embora a velocidade de recuperação não seja a principal meta deste guia, você pode alcançar tempos de recuperação menores:

- Criando um plano de recuperação de floresta detalhado, atualizando-o regularmente e praticando-o em um ambiente de teste simulado de tamanho razoável pelo menos uma vez por ano
- Usando a clonagem do DC (controlador de domínio virtualizado)
   - A clonagem de DC virtualizado acelera o processo para obter DCs adicionais em execução depois que um DC é restaurado do backup em cada domínio. Os DCs virtualizados adicionais podem ser clonados em vez de esperar que instalações de AD DS potencialmente demoradas sejam concluídas e para a conclusão da replicação não crítica após a instalação.
   - As florestas em que os DCs virtuais são hospedados em um número relativamente pequeno de data centers bem conectados podem beneficiar a maior parte da clonagem durante a recuperação. No entanto, qualquer ambiente em que vários DCs virtualizados para o mesmo domínio está colocalizado no mesmo host de hipervisor deve se beneficiar.
- Implantando RODCs (controladores de domínio somente leitura)
   - Os RODCs podem fornecer continuidade de negócios durante o processo de recuperação porque eles não precisam ser desconectados da rede como DCs graváveis. Os RODCs não executam a replicação de saída. Portanto, eles não apresentam o mesmo risco que os DCs graváveis representam para replicar dados prejudiciais de volta para o ambiente recuperado.

Outros fatores que afetam a duração do processo de recuperação de floresta incluem o seguinte:

- Quando você restaura os DCs de backups, leva tempo para:
   - Localize a mídia de backup física, como fitas.
   - Reinstale o sistema operacional.
   - Restaurar dados da mídia de backup.
      - Você pode reduzir o tempo necessário para reinstalar o sistema operacional e restaurar os dados do backup executando a recuperação completa do servidor em vez da restauração do estado do sistema. Como a recuperação completa do servidor é baseada em binário, ela é concluída muito mais rapidamente do que a restauração do estado do sistema.
      - No entanto, se o servidor contiver dados que são excluídos dos dados de estado do sistema que você não deseja restaurar, a recuperação de servidor completo poderá não ser uma alternativa viável para a restauração do estado do sistema. Considere as vantagens de executar uma recuperação de servidor completa em vez de uma restauração de estado do sistema para seus servidores de forma específica e prepare-se de acordo com a execução do tipo apropriado de backup que você planeja restaurar posteriormente.
- Quando você recria DCs, leva tempo para replicar dados para promoções baseadas em rede.
   - Você pode diminuir o tempo necessário para restaurar os DCs executando as seguintes etapas:
- Reduza o tempo de recuperação de mídia de backup:
   - Usando a ferramenta de montagem de banco de dados Active Directory (Dsamain.exe) para identificar o melhor backup a ser usado para operações de restauração. Para obter mais informações sobre como usar a ferramenta de montagem de banco de dados Active Directory, consulte o [guia passo a passo da ferramenta de montagem de banco de dados do Active Directory](https://go.microsoft.com/fwlink/?LinkId=132577) ( https://go.microsoft.com/fwlink/?LinkId=132577) .
   - Rotular a mídia de backup de forma clara e armazenar a mídia de maneira organizada em um local conveniente, embora seguro, que permita a recuperação rápida.
   - Usar o Serviço de Cópias de Sombra de Volume com uma SAN (rede de área de armazenamento) para manter backups de diferentes pontos no tempo. Para obter mais informações, consulte [Windows Server 2003 Active Directory recuperação rápida com serviço de cópias de sombra de volume e serviço de disco virtual](https://go.microsoft.com/fwlink/?LinkId=70781) ( https://go.microsoft.com/fwlink/?LinkId=70781) .
- Force a remoção de AD DS dos DCs em vez de reinstalar o sistema operacional. Se a causa da falha em toda a floresta tiver sido identificada como puramente dentro do escopo de AD DS, você não precisará reinstalar o sistema operacional nos controladores de domínio.
   - Para obter mais informações sobre como forçar a remoção de AD DS de um DC que executa o Windows Server 2008 ou posterior, consulte [forçando a remoção de um controlador de domínio do Windows server 2008](https://go.microsoft.com/fwlink/?LinkId=132627) ( https://go.microsoft.com/fwlink/?LinkId=132627) . Para obter mais informações sobre como forçar a remoção de AD DS de um DC que executa o Windows Server 2003, consulte o [artigo 332199](https://go.microsoft.com/fwlink/?LinkId=70780) na base de dados de conhecimento Microsoft ( https://go.microsoft.com/fwlink/?LinkId=70780) .
- Use dispositivos de fita ou backups de disco mais rápidos para reduzir o tempo necessário para operações de restauração.

Você também pode ajudar a acelerar AD DS instalações usando o recurso IFM (instalar da mídia) para recriar DCs em cada domínio. A IFM reduz a latência de replicação incorrida quando você recria os DCs em cada domínio.

As empresas que têm um SLA (contrato de nível de serviço) mais agressivo podem considerar a alteração dos procedimentos de recuperação de floresta para acelerar a recuperação.

**P: posso automatizar o processo de recuperação de floresta?**

Devido à natureza complexa e crítica do processo de recuperação de floresta, atualmente, não há nenhuma automação de ponta a ponta. O processo de recuperação de floresta é mais um desafio logístico e organizacional de restaurar a continuidade dos negócios do que um problema técnico de automação de processos. Portanto, o indivíduo que administra o ambiente deve criar um plano de recuperação de floresta específico para esse ambiente e, em seguida, automatizar as seções de ti que podem ser automatizadas com êxito.

Você pode executar a maioria das etapas de recuperação de floresta usando ferramentas de linha de comando. Portanto, a maioria das etapas é programável por script. Por exemplo, Ntdsutil.exe é uma das ferramentas usadas com mais frequência no processo de recuperação de floresta.

Embora os scripts possam acelerar a recuperação, você deve testar exaustivamente esses scripts antes de aplicá-los em um ambiente real. Além disso, você deve atualizá-los de acordo com as alterações no ambiente de Active Directory, como a adição de um novo domínio ou DC, ou uma nova versão do Active Directory.

## <a name="next-steps"></a>Próximas etapas

- [Recuperação de floresta do AD – Pré-requisitos](AD-Forest-Recovery-Prerequisties.md)
- [Recuperação de floresta do AD-planejar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)
- [Recuperação de floresta do AD – identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD-determine como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD-executar recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
- [Recuperação de floresta do AD-perguntas frequentes](AD-Forest-Recovery-FAQ.md)
- [Recuperação de floresta do AD-recuperando um único domínio em uma floresta de multidomínio](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [Recuperação de floresta do AD-recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
