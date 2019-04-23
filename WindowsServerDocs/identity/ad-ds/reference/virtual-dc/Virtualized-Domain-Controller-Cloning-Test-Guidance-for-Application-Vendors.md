---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: Diretrizes do teste de clonagem do controlador de domínio virtualizado para fornecedores de aplicativos
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b2303bc837cdaf9f6e7ebd4b3ccbf6c66aa7ad2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879337"
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>Diretrizes do teste de clonagem do controlador de domínio virtualizado para fornecedores de aplicativos

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica o que os fornecedores de aplicativos devem considerar para ajudar a garantir que seu aplicativo continua a funcionar como esperado depois que o controlador de domínio virtualizado (DC) com o processo de clonagem for concluída. Ele aborda os aspectos do processo de clonagem que fornecedores de aplicativos de seu interesse e cenários que podem exigir testes adicionais. Fornecedores de aplicativos que validar que seu aplicativo funciona em controladores de domínio virtualizado que foram clonados são incentivados a listar o nome do aplicativo no conteúdo da comunidade na parte inferior deste tópico, juntamente com um link para seu site da organização em que os usuários possam saber mais sobre a validação.  
  
## <a name="overview-of-virtualized-dc-cloning"></a>Visão geral da clonagem do controlador de domínio virtualizado  
O processo de clonagem do controlador de domínio virtualizado é descrito detalhadamente no [Introdução à virtualização de serviços de domínio Active Directory (AD DS) (nível 100)](https://technet.microsoft.com/library/hh831734.aspx) e [virtualizado do técnico controlador de domínio Referência (nível 300)](https://technet.microsoft.com/library/jj574214.aspx). Da perspectiva do fornecedor do aplicativo, estas são algumas considerações para levar em conta ao avaliar o impacto de clonagem para seu aplicativo:  
  
-   O computador original não será destruído. Ele permanece na rede, interagir com os clientes. Ao contrário de uma renomeação em que os registros DNS do computador original são removidos, os registros originais para o controlador de domínio de origem permanecem.  
  
-   Durante o processo de clonagem, o novo computador é inicialmente em execução por um breve período de tempo sob a identidade do computador antigo até que o processo de clonagem é iniciado e faz as alterações necessárias. Aplicativos que criam registros sobre o host devem garantir que o computador clonado não substituirá os registros sobre o host original durante o processo de clonagem.  
  
-   A clonagem é uma funcionalidade de implantação específica para controladores de domínio virtualizado apenas, não uma extensão de finalidade geral para clonar a outras funções de servidor. Algumas funções de servidor especificamente não têm suporte para clonagem:  
  
    -   Protocolo DHCP  
  
    -   Serviços de certificados do Active Directory (AD CS)  
  
    -   AD LDS (Active Directory Lightweight Directory Services)  
  
-   Como parte do processo de clonagem, toda a VM que representa o controlador de domínio original for copiada, portanto, qualquer estado de aplicativo nessa VM também é copiado. Validar que o aplicativo se adapta a essa alteração no estado do host local no controlador de domínio clonado, ou se nenhuma intervenção é necessária, como uma reinicialização do serviço.  
  
-   Como parte da clonagem, o novo controlador de domínio obtém uma nova identidade da máquina e provisiona em si como uma réplica de controlador de domínio na topologia. Valide se o aplicativo depende da identidade da máquina, como seu nome, conta, SID e assim por diante. -Lo se adaptam automaticamente à alteração de identidade da máquina no clone? Se esse aplicativo armazena em cache os dados, certifique-se de que ele não se baseia em dados de identidade da máquina que podem ser armazenados em cache.  
  
## <a name="what-is-interesting-for-application-vendors"></a>O que é interessante para fornecedores de aplicativos?  
  
### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml  
Não é possível clonar o controlador de domínio que executa o aplicativo ou serviço até que o aplicativo ou serviço é:  
  
-   Adicionado ao arquivo customdccloneallowlist. XML usando o cmdlet Get-ADDCCloningExcludedApplicationList Windows PowerShell  
  
-Or-  
  
-   Removido do controlador de domínio  
  
Na primeira vez em que o usuário executa o cmdlet Get-ADDCCloningExcludedApplicationList, ele retorna uma lista de serviços e aplicativos que estão em execução no controlador de domínio, mas que não estão na lista padrão de serviços e aplicativos que têm suporte para clonagem. Por padrão, seu serviço ou aplicativo não será listado. Para adicionar o seu serviço ou aplicativo à lista de aplicativos e serviços que podem ser ignorado com segurança clonado, as cmdlet Get-ADDCCloningExcludedApplicationList novamente com a opção – GenerateXML para adicioná-lo ao customdccloneallowlist. XML do arquivo de execuções de usuário. Para obter mais informações, consulte [etapa 2: Execute o cmdlet Get-ADDCCloningExcludedApplicationList](https://technet.microsoft.com/library/hh831734.aspx#bkmk6_run_get_addccloningexcludedapplicationlist_cmdlet).  
  
### <a name="distributed-system-interactions"></a>Interações de sistema distribuído  
Normalmente, serviços isolados no computador local são aprovados ou reprovados ao participar de clonagem. Serviços distribuídos precisam se preocupar com duas instâncias do computador host na rede simultaneamente por um breve período de tempo. Isso pode se manifestar como uma instância de serviço tentando extrair informações de um sistema de parceiro em que o clone foi registrado como o novo fornecedor da identidade. Ou ambas as instâncias do serviço podem enviar informações para o banco de dados do AD DS ao mesmo tempo com resultados diferentes. Por exemplo, ele não é determinística qual computador será comunicada com quando dois computadores que têm o serviço de tecnologias de teste do Windows (WTT) estão na rede com o controlador de domínio.  
  
Para o serviço servidor DNS distribuído, o processo de clonagem cuidadosamente impede os registros DNS do controlador de domínio de origem quando o controlador de domínio do clone é iniciado com um novo endereço IP.  
  
Você não deve confiar no computador para remover todos da identidade do antiga até o término da clonagem. Depois que o novo controlador de domínio é promovido dentro do contexto de novo, selecione Sysprep provedores são executados para limpar o estado adicional do computador. Por exemplo, neste ponto é os antigos certificados do computador são removidos e os segredos de criptografia que o computador pode acessar são alterados.  
  
O maior fator que varia o controle de tempo da clonagem é quantos objetos são replicar a partir do controlador de domínio primário. Mídia mais antiga aumenta o tempo necessário para clonagem concluída.  
  
Como o seu serviço ou aplicativo é desconhecido, é deixado em execução. O processo de clonagem não altera o estado dos serviços não-Windows.  
  
Além disso, o novo computador tem um endereço IP diferente do computador original. Esses comportamentos podem causar efeitos colaterais para seu serviço ou aplicativo, dependendo de como o serviço ou aplicativo se comporta nesse ambiente.  
  
## <a name="additional-scenarios-suggested-for-testing"></a>Cenários adicionais sugeridos para teste  
  
### <a name="cloning-failure"></a>Falha na clonagem  
Fornecedores de serviço devem testar esse cenário porque quando a clonagem falha o computador será inicializado em diretório de serviços de reparo DSRM (modo), um formulário do modo de segurança. Neste ponto o computador não foi concluída de clonagem. Algum estado pode ter sido alterado e algum estado pode permanecer do controlador de domínio original. Teste este cenário para entender o impacto que ela pode ter em seu aplicativo.  
  
Para induzir uma falha de clonagem, tente clonar um controlador de domínio sem conceder a ele permissão a ser clonado. Nesse caso, o computador será apenas alterou os endereços IP e ainda tem a maior parte do seu estado do controlador de domínio original. Para obter mais informações sobre como conceder uma permissão de controlador de domínio a ser clonado, consulte [etapa 1: Conceder a permissão a ser clonado do controlador de domínio virtualizado de origem](https://technet.microsoft.com/library/hh831734.aspx#bkmk4_grant_source).  
  
### <a name="pdc-emulator-cloning"></a>Emulador do PDC de clonagem  
Fornecedores de serviço e do aplicativos devem testar esse cenário porque há uma reinicialização adicional quando o emulador PDC é clonado. Além disso, a maioria de clonagem seja executada sob uma identidade temporária para permitir que o novo clone interagir com o emulador do PDC durante o processo de clonagem.  
  
### <a name="writable-versus-read-only-domain-controllers"></a>Gravável em comparação com controladores de domínio somente leitura  
Fornecedores de serviço e do aplicativos devem testar a clonagem, usando o mesmo tipo de controlador de domínio (ou seja, em um controlador de domínio gravável ou somente leitura) que o serviço é planejado para ser executado no.  
  


