---
title: "Recuperação de floresta do AD - perguntas Frequentes"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adfs
ms.openlocfilehash: 839e01c88b7ced22501154d8752c6c71cc52b696
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---faq"></a>Recuperação de floresta do AD - perguntas Frequentes

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2, Windows Server 2003

Este documento contém perguntas frequentes perguntas frequentes sobre a recuperação de floresta:  
 
## <a name="general-recovery"></a>Gerais de recuperação
  
 
**P: o que pode fazer para acelerar a recuperação?** 
 
Embora a velocidade de recuperação não é o principal objetivo deste guia, você pode obter menores tempos de recuperação por:  
  
-   Criando um plano de recuperação de floresta detalhadas, atualizando regularmente e praticando-lo em um ambiente de teste simulado de tamanho razoável pelo menos uma vez por ano  
  
-   Uso da clonagem do domínio virtualizado controlador (DC)  
  
     Virtualizado DC clonagem agiliza o processo para obter os controladores de domínio adicionais em execução depois que um que controlador de domínio é restaurado de backup em cada domínio. Os controladores de domínio virtualizados adicionais podem ser clonados em vez de aguardar potencialmente demorada AD DS instalações ser concluído e para a conclusão da replicação não críticas após a instalação.  
  
     Florestas onde virtuais controladores de domínio são hospedados em um número relativamente pequeno de datacenters bem conectados potencialmente aproveitar clonagem durante a recuperação. No entanto, qualquer ambiente onde vários controladores de domínio virtualizados para o mesmo domínio estão localizadas no mesmo hipervisor host deve tirar proveito.  
  
-   Implantando controladores de domínio somente leitura (RODCs)  
  
     RODCs podem fornecer continuidade de negócios durante o processo de recuperação porque eles não precisam ser desconectado da rede, como controladores de domínio graváveis. RODCs não execute a duplicação de saída. Portanto, eles não apresentam o mesmo risco que representam controladores de domínio graváveis para replicação prejudiciais dados de volta para o ambiente recuperado.  
  
 Outros fatores que afetam a duração do processo de recuperação de floresta incluem o seguinte:  
  
-   Quando você restaurar controladores de domínio de backups, demoram para:  
  
    -   Localize a mídia de backup física, como fitas.  
  
    -   Reinstale o sistema operacional.  
  
    -   Restaure dados de mídia de backup.  
  
     Você pode reduzir o tempo necessário para reinstalar o sistema operacional e restaurar dados de backup realizando a recuperação de servidor completo em vez de restauração do estado do sistema. Como recuperação de servidor completo é baseada em binário, ele conclui muito rapidamente do que a restauração do estado do sistema.  
  
     No entanto, se o servidor contém dados que são excluídos de dados de estado do sistema que você não deseja restaurar, recuperação completa do servidor não pode ser uma alternativa viável para restauração do estado do sistema. Considere as vantagens de realizar uma recuperação de servidor completo em vez de uma restauração de estado do sistema para seus servidores especificamente e preparar adequadamente realizando o tipo adequado de backup que você pretende restaurar posteriormente.  
  
-   Ao recriar controladores de domínio, ele leva tempo para replicar dados para promoções com base em rede.  
  
 Você pode reduzir o tempo necessário para restaurar controladores de domínio executando as seguintes etapas:  
  
-   Reduza o tempo para a recuperação de mídia de backup por:  
  
    -   Usando a ferramenta de montagem de banco de dados do Active Directory (Dsamain.exe) para identificar o backup melhor a ser usado para operações de restauração. Para obter mais informações sobre como usar o banco de dados de montagem ferramenta do Active Directory, consulte o [ferramenta de montagem banco de dados do Active Directory Step-by-Step guia](https://go.microsoft.com/fwlink/?LinkId=132577) (https://go.microsoft.com/fwlink/?LinkId=132577).  
  
    -   Rotulando claramente a mídia de backup e armazenar a mídia em um modo organizado em uma conveniente, ainda, local seguro que permite a recuperação rápida.  
  
    -   Usando o serviço de cópias de sombra de Volume com uma rede de área de armazenamento (SAN) para manter os backups de pontos diferentes no momento. Para obter mais informações, consulte [Windows Server 2003 Active Directory recuperação rápida com o serviço de cópias de sombra de Volume e o serviço de disco Virtual](https://go.microsoft.com/fwlink/?LinkId=70781) (https://go.microsoft.com/fwlink/?LinkId=70781).  
  
-   Força a remoção do AD DS de controladores de domínio em vez de reinstalar o sistema operacional. Se a causa da falha de toda a floresta tiver sido identificada para ser puramente dentro do escopo do AD DS, você não precisa reinstalar o sistema operacional em controladores de domínio.  
  
     Para obter mais informações sobre como forçar a remoção do AD DS de um controlador de domínio que executa o Windows Server 2008 ou posterior, veja [forçando a remoção de um controlador de domínio do Windows Server 2008](https://go.microsoft.com/fwlink/?LinkId=132627) (https://go.microsoft.com/fwlink/?LinkId=132627). Para obter mais informações sobre como forçar a remoção do AD DS de um controlador de domínio que executa o Windows Server 2003, consulte [332199 do artigo](https://go.microsoft.com/fwlink/?LinkId=70780) na Base de dados de Conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=70780).  
  
-   Use dispositivos fita mais rápidos ou backups de disco para reduzir o tempo necessário para restaurar as operações.  
  
 Você também pode ajudar a acelerar instalações do AD DS, usando a instalação do recurso de mídia (IFM) para recriar os controladores de domínio em cada domínio. IFM reduz a latência de replicação é incorrida ao recriar controladores de domínio em cada domínio.  
  
 As empresas que têm um contrato de nível de serviço (SLA) mais agressivo considere alterar os procedimentos de recuperação de floresta para acelerar a recuperação.  
  

**P: posso automatizar o processo de recuperação de floresta?**  
 Devido à natureza complexa e essenciais do processo de recuperação de floresta, não há atualmente nenhum automação de ponta a ponta dele. O processo de recuperação de floresta é mais um desafio logístico e organização de continuidade de negócios que um problema técnico de automação do processo de restauração. Portanto, a pessoa que administra o ambiente deve criar um plano de recuperação de floresta que é específico para esse ambiente e automatizar as suas seções que podem ser automatizadas com êxito.  
  
 Você pode realizar a maioria das etapas de recuperação de floresta usando as ferramentas de linha de comando. Portanto, a maioria das etapas são passíveis de script. Por exemplo, Ntdsutil.exe é uma das ferramentas usadas com mais frequência no processo de recuperação de floresta.  
  
 Embora scripts podem acelerar a recuperação, você deve testar exaustivamente esses scripts antes de aplicá-los em um ambiente real. Além disso, será necessário atualizá-los acordo com as alterações no ambiente do Active Directory, como a adição de um novo domínio ou controlador de domínio ou uma nova versão do Active Directory.

## <a name="next-steps"></a>Próximas etapas
-   [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperação de floresta do AD - descobrindo um plano de recuperação personalizada floresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperação de floresta do AD - realizar uma recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperação de floresta do AD - recuperando um único domínio em uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [AD floresta recuperação - floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
