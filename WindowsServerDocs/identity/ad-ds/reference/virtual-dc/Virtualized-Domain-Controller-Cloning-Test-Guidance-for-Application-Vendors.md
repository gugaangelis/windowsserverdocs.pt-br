---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: "Controlador de domínio virtualizado clonagem diretriz de teste para fornecedores de aplicativos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 72c4e818f82d3252c45776b26fb59e095893f2c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>Controlador de domínio virtualizado clonagem diretriz de teste para fornecedores de aplicativos

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica o que os fornecedores de aplicativos devem considerar para garantir que seu aplicativo continua a funcionar como esperado após a conclusão do controlador de domínio virtualizado (DC) processo de clonagem. Ele aborda os aspectos do processo de clonagem que fornecedores de aplicativos de interesse e cenários que podem garantir testes adicionais. Fornecedores de aplicativo que tem validado que seu aplicativo funciona em controladores de domínio virtualizado que tenham sido clonados são incentivados a listar o nome do aplicativo no conteúdo da comunidade na parte inferior deste tópico, juntamente com um link para o site da sua organização onde os usuários podem saber mais sobre a validação.  
  
## <a name="overview-of-virtualized-dc-cloning"></a>Visão geral da clonagem virtualizada de DC  
O controlador de domínio virtualizado clonagem processo é descrito em detalhes no [Introdução aos serviços de domínio do Active Directory (AD DS) virtualização (nível 100)](https://technet.microsoft.com/library/hh831734.aspx) e [virtualizados domínio controlador referência técnica (nível 300)](https://technet.microsoft.com/library/jj574214.aspx). Da perspectiva do fornecedor do aplicativo, estas são algumas considerações para levar em consideração ao avaliar o impacto de clonagem para seu aplicativo:  
  
-   O computador original não será destruído. Ele permanece na rede, interagir com clientes. Ao contrário de uma alteração de nome onde os registros de DNS do computador original são removidos, permanecem os registros originais para o controlador de domínio de origem.  
  
-   Durante o processo de clonagem, o novo computador está inicialmente em execução por um breve período de tempo com a identidade do computador antigo até que o processo de clonagem for iniciado e faz as alterações necessárias. Aplicativos que criam registros sobre o host devem garantir que o computador clonado não substituir registros sobre o host original durante o processo de clonagem.  
  
-   Clonagem é uma funcionalidade de implantação específica somente virtualizado para controladores de domínio, não uma extensão de finalidade geral para clonar outras funções de servidor. Algumas funções de servidor especificamente não são suportadas para clonagem:  
  
    -   Protocolo DHCP (protocolo DHCP)  
  
    -   Serviços de certificados do Active Directory (AD CS)  
  
    -   No Active Directory Lightweight Directory Services (AD LDS)  
  
-   Como parte do processo de clonagem, a VM inteira que representa o controlador de domínio original é copiada, portanto, qualquer estado do aplicativo em que VM também é copiado. Validar que o aplicativo se adapta a essa alteração no estado do host local no controlador de domínio clonado, ou se nenhuma intervenção for necessária, como reiniciar o serviço.  
  
-   Como parte de clonagem, o novo DC ganha uma nova identidade do computador e disposições em si como uma réplica DC na topologia. Valide se o aplicativo depende da identidade do computador, como seu nome, conta, SID e assim por diante. -Lo automaticamente se adaptar a alteração de identidade do computador no clone? Se esse aplicativo armazena dados em cache, certifique-se de que ele não se baseia em dados de identidade do computador que podem ser armazenados em cache.  
  
## <a name="what-is-interesting-for-application-vendors"></a>O que é interessante para fornecedores de aplicativos?  
  
### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml  
Um controlador de domínio que executa o aplicativo ou serviço não pode ser clonado até que o aplicativo ou serviço é:  
  
-   Adicionado ao arquivo CustomDCCloneAllowList.xml usando o cmdlet Get-ADDCCloningExcludedApplicationList Windows PowerShell  
  
- Ou -  
  
-   Removido o controlador de domínio  
  
A primeira vez em que o usuário executa o cmdlet Get-ADDCCloningExcludedApplicationList, ele retorna uma lista de serviços e aplicativos que estão em execução no controlador de domínio, mas não estão na lista padrão de serviços e aplicativos que têm suporte para clonagem. Por padrão, seu aplicativo ou serviço não aparecerá. Para adicionar seu serviço ou aplicativo à lista de aplicativos e serviços que podem ser com segurança clonados, a execução de usuário cmdlet Get-ADDCCloningExcludedApplicationList novamente com a opção - GenerateXML para adicioná-lo para o CustomDCCloneAllowList.xml do arquivo. Para obter mais informações, consulte [etapa 2: cmdlet Get-ADDCCloningExcludedApplicationList executar](https://technet.microsoft.com/library/hh831734.aspx#bkmk6_run_get_addccloningexcludedapplicationlist_cmdlet).  
  
### <a name="distributed-system-interactions"></a>Interações do sistema distribuído  
Geralmente serviços isolados no computador local ou passam ou falharem ao participar de clonagem. Serviços distribuídos precisam se preocupar com duas instâncias do computador host na rede simultaneamente por um breve período de tempo. Isso pode ocorrer como uma instância de serviço tentando transmitem as informações de um sistema de parceiro onde o clone registrou como o novo fornecedor da identidade. Ou ambas as instâncias do serviço podem enviar informações para o banco de dados do AD DS ao mesmo tempo com resultados diferentes. Por exemplo, não é determinística qual computador será divulgado com quando dois computadores com o serviço de tecnologias de testes do Windows (WTT) estão na rede com o controlador de domínio.  
  
Para o serviço de servidor DNS distribuído, o processo de clonagem cuidadosamente evita substituam os registros de DNS do controlador de domínio de origem quando o controlador de domínio clone começa com um novo endereço IP.  
  
Você não deve depender do computador para remover todos a identidade antiga até o final de clonagem. Depois que o novo controlador de domínio for promovido dentro do contexto de novo, selecione Sysprep provedores são executados para limpar o estado adicional do computador. Por exemplo, nesse ponto é os certificados do computador antigos são removidos e os segredos de criptografia que o computador pode acessar são alterados.  
  
O maior fator que varia o tempo de clonagem é quantos objetos são replicar do PDC. Mídia mais antiga aumenta o tempo necessário para clonagem completa.  
  
Como seu aplicativo ou serviço é reconhecido, é deixado em execução. O processo de clonagem não altera o estado dos serviços não são do Windows.  
  
Além disso, o novo computador tem um endereço IP diferente do que o computador original. Esses comportamentos podem causar efeitos colaterais para seu serviço ou aplicativo dependendo de como o serviço ou aplicativo se comporta nesse ambiente.  
  
## <a name="additional-scenarios-suggested-for-testing"></a>Cenários adicionais sugeridos para testes  
  
### <a name="cloning-failure"></a>Falha de clonagem  
Fornecedores de serviço devem testar esse cenário porque quando clonagem falha no diretório serviços de reparo modo (DSRM), uma forma de modo de segurança de inicialização do computador. Agora o computador não for concluída clonagem. Algum estado pode ter mudado e pode permanecer algum estado do controlador de domínio original. Este cenário para entender o que o impacto que ele pode ter em seu aplicativo de teste.  
  
Para induzir uma falha de clonagem, tente clonar um controlador de domínio sem conceder permissão para ser clonados a ele. Nesse caso, o computador terá somente sido alterado os endereços IP e ainda ter a maior parte do estado do controlador de domínio original. Para obter mais informações sobre como conceder uma permissão do controlador de domínio para ser clonados, consulte [etapa 1: conceder o controlador de domínio virtualizado origem a permissão para ser clonados](https://technet.microsoft.com/library/hh831734.aspx#bkmk4_grant_source).  
  
### <a name="pdc-emulator-cloning"></a>Emulador do PDC clonagem  
Fornecedores de serviço e o aplicativos devem testar esse cenário porque não há uma reinicialização adicional quando o emulador do PDC é clonado. Além disso, a maioria dos clonagem é executada sob uma identidade temporária para permitir que o novo clone interagir com o emulador do PDC durante o processo de clonagem.  
  
### <a name="writable-versus-read-only-domain-controllers"></a>Gravável versus controladores de domínio somente leitura  
Fornecedores de serviço e o aplicativos devem testar clonagem usando o mesmo tipo de controlador de domínio (ou seja, em um controlador de domínio gravável ou somente leitura) que o serviço está programado para ser executado em.  
  


