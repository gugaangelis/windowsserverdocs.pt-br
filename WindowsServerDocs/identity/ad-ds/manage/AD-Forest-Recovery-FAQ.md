---
title: Recuperação de floresta do AD - perguntas Frequentes
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adds
ms.openlocfilehash: 36c9560b490cc28f006770c869bdcdf2b152dd74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878447"
---
# <a name="ad-forest-recovery---faq"></a>Recuperação de floresta do AD - perguntas Frequentes

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2, Windows Server 2003

Este documento contém perguntas frequentes (FAQs) sobre recuperação de floresta:  

## <a name="general-recovery"></a>Gerais de recuperação

**P: O que pode fazer para aumentar a velocidade de recuperação?**

Embora a velocidade de recuperação não é o objetivo principal deste guia, você pode obter tempos de recuperação menores por:  
  
- Criando um plano de recuperação de floresta detalhada, atualizá-lo com regularidade e praticando-lo em um ambiente de teste simulado de tamanho razoável pelo menos uma vez por ano  
- Usando a clonagem de DC (controlador) de domínio virtualizado  
   - Clonagem de controlador de domínio virtualizado acelera o processo para obter os controladores de domínio adicionais executando depois que um que controlador de domínio é restaurado do backup em cada domínio. Os controladores de domínio virtualizados adicionais podem ser clonados em vez de aguardar conclusão das instalações potencialmente demorado AD DS e a conclusão da replicação não crítica após a instalação.  
   - Florestas onde controladores de domínio virtuais hospedados em um número relativamente pequeno de centros de dados bem conectada potencialmente mais durante a recuperação de clonagem beneficiam. No entanto, qualquer ambiente no qual vários controladores de domínio virtualizados para o mesmo domínio estiverem localizados em conjunto no mesmo host de hipervisor deve se beneficiar.  
- Implantação de controladores de domínio somente leitura (RODCs)  
   - Os RODCs podem fornecer continuidade dos negócios durante o processo de recuperação pois eles não precisam ser desconectado da rede, assim como os controladores de domínio graváveis. Os RODCs não realize a replicação de saída. Portanto, eles não apresentam o mesmo risco que os controladores de domínio graváveis apresentam para replicar dados prejudiciais novamente para o ambiente recuperado.  
  
Outros fatores que afetam a duração do processo de recuperação de floresta incluem o seguinte:  
  
- Quando você restaurar controladores de domínio dos backups, leva tempo para:  
   - Localize a mídia de backup física, como fitas.  
   - Reinstale o sistema operacional.  
   - Restaure os dados da mídia de backup.  
      - Você pode reduzir o tempo necessário para reinstalar o sistema operacional e restaurar dados de backup ao executar a recuperação de servidor completo em vez de restauração de estado do sistema. Como a recuperação de servidor completo é baseada em binário, ele será concluído muito mais rapidamente do que a restauração de estado do sistema.  
      - No entanto, se o servidor contém dados que são excluídos de dados de estado do sistema que você não deseja restaurar, recuperação de servidor completo não pode ser uma alternativa viável para restauração de estado do sistema. Considere as vantagens de executar uma recuperação de servidor completo em vez de uma restauração de estado do sistema para seus servidores especificamente e preparar adequadamente, executando o tipo adequado de backup que você planeja restaurar mais tarde.  
- Quando você recria os controladores de domínio, ele leva tempo para replicar dados para as promoções baseadas em rede.  
   - Você pode diminuir o tempo necessário para restaurar controladores de domínio executando as seguintes etapas:  
- Reduza o tempo de recuperação de mídia de backup por meio de:  
   - Usando a ferramenta de montagem do banco de dados de diretório Active Directory (Dsamain.exe) para identificar o melhor de backup a ser usado para operações de restauração. Para obter mais informações sobre como usar a ferramenta Active Directory banco de dados de montagem, consulte o [Active Directory banco de dados montando ferramenta guia passo a passo](https://go.microsoft.com/fwlink/?LinkId=132577) (https://go.microsoft.com/fwlink/?LinkId=132577).  
   - Rotulação da mídia de backup claramente e armazenar a mídia de modo organizado em conveniente, ainda, local seguro que permite a recuperação rápida.  
   - Usando o serviço de cópias de sombra de Volume com uma rede de área de armazenamento (SAN) para manter os backups de diferentes pontos no tempo. Para obter mais informações, consulte [Windows Server 2003 rápida recuperação do Active Directory com o serviço de cópias de sombra de Volume e o serviço de disco Virtual](https://go.microsoft.com/fwlink/?LinkId=70781) (https://go.microsoft.com/fwlink/?LinkId=70781).  
- Força a remoção do AD DS de controladores de domínio em vez de reinstalar o sistema operacional. Se a causa da falha de toda a floresta tiver sido identificada para ser puramente dentro do escopo do AD DS, não é necessário reinstalar o sistema operacional nos controladores de domínio.  
   - Para obter mais informações sobre como forçar a remoção do AD DS de um controlador de domínio que executa o Windows Server 2008 ou posterior, consulte [forçar a remoção de um controlador de domínio do Windows Server 2008](https://go.microsoft.com/fwlink/?LinkId=132627) (https://go.microsoft.com/fwlink/?LinkId=132627). Para obter mais informações sobre como forçar a remoção do AD DS de um controlador de domínio que executa o Windows Server 2003, consulte [332199 do artigo](https://go.microsoft.com/fwlink/?LinkId=70780) na Base de dados de Conhecimento Microsoft (https://go.microsoft.com/fwlink/?LinkId=70780).  
- Usar dispositivos de fita mais rápidos ou backups de disco para reduzir o tempo necessário para operações de restauração.  
  
Você também pode ajudar a acelerar as instalações do AD DS usando a instalação do recurso de mídia (IFM) para recompilar os controladores de domínio em cada domínio. IFM reduz a latência de replicação é incorrida quando você recria os controladores de domínio em cada domínio.  
  
Empresas que têm um contrato de nível de serviço (SLA) mais agressivo podem considerar alterando os procedimentos de recuperação de floresta para acelerar a recuperação.  
  
**P: Posso automatizar o processo de recuperação de floresta?**

Devido à natureza complexa e crítica do processo de recuperação de floresta, atualmente, não há nenhuma automação de ponta a ponta dele. O processo de recuperação de floresta é mais um desafio de logístico e organizacional de continuidade de negócios que um problema técnico de automação de processo de restauração. Portanto, a pessoa que administra o ambiente deve criar um plano de recuperação de floresta que é específico para o ambiente e automatizar as suas seções que podem ser automatizadas com êxito.  
  
Você pode executar a maioria das etapas de recuperação de floresta usando as ferramentas de linha de comando. Portanto, a maioria das etapas é programáveis por scripts. Por exemplo, Ntdsutil.exe é uma das ferramentas mais usadas no processo de recuperação de floresta.  
  
Embora os scripts podem acelerar a recuperação, você deve testar exaustivamente esses scripts antes de aplicá-las em um ambiente real. Além disso, você deve atualizá-los de acordo com a alterações no ambiente do Active Directory, como a adição de um novo domínio ou controlador de domínio ou uma nova versão do Active Directory.

## <a name="next-steps"></a>Próximas etapas

- [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperação de floresta do AD - elaborar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD - executar a recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
- [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperação de floresta do AD - recuperação de um único domínio dentro de uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperação de floresta do AD - recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
