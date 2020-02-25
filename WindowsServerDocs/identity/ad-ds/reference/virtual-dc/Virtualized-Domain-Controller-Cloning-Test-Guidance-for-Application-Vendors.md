---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: Diretrizes do teste de clonagem do controlador de domínio virtualizado para fornecedores de aplicativos
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 4926fabe255f964b6d39e6c39c5e794a37423111
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77517461"
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>Diretrizes do teste de clonagem do controlador de domínio virtualizado para fornecedores de aplicativos

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica o que os fornecedores de aplicativos devem considerar para ajudar a garantir que seu aplicativo continue funcionando conforme o esperado após a conclusão do processo de clonagem do DC (controlador de domínio virtualizado). Ele aborda esses aspectos do processo de clonagem que interessam os fornecedores de aplicativos e cenários que podem garantir testes adicionais. Os fornecedores de aplicativos que validaram que seus aplicativos funcionam em controladores de domínio virtualizados que foram clonados são incentivados a listar o nome do aplicativo no conteúdo da Comunidade, na parte inferior deste tópico, junto com um link para seu site da organização onde os usuários podem saber mais sobre a validação.

## <a name="overview-of-virtualized-dc-cloning"></a>Visão geral da clonagem de DC virtualizado
O processo de clonagem do controlador de domínio virtualizado é descrito em detalhes na [introdução à virtualização de Active Directory Domain Services (AD DS) (nível 100) e à](https://docs.microsoft.com/windows-server/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) [referência técnica do controlador de domínio virtualizado (nível 300)](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-). Da perspectiva de um fornecedor de aplicativos, estas são algumas considerações a serem levadas em conta ao avaliar o impacto da clonagem para seu aplicativo:

-   O computador original não é destruído. Ele permanece na rede, interagindo com clientes. Ao contrário de uma renomeação onde os registros DNS do computador original são removidos, os registros originais do controlador de domínio de origem permanecem.

-   Durante o processo de clonagem, o novo computador estará inicialmente em execução por um breve período de tempo sob a identidade do computador antigo até que o processo de clonagem seja iniciado e faça as alterações necessárias. Os aplicativos que criam registros sobre o host devem garantir que o computador clonado não substitua os registros sobre o host original durante o processo de clonagem.

-   A clonagem é um recurso de implantação específico somente para controladores de domínio virtualizados, não uma extensão de finalidade geral para clonar outras funções de servidor. Algumas funções de servidor não têm suporte especificamente para clonagem:

    -   Protocolo DHCP

    -   AD CS (Serviços de Certificados do Active Directory)

    -   AD LDS (Active Directory Lightweight Directory Services)

-   Como parte do processo de clonagem, toda a VM que representa o DC original é copiada, portanto, qualquer estado do aplicativo nessa VM também é copiado. Valide se o aplicativo se adapta a essa alteração no estado do host local no controlador de domínio clonado ou se é necessária alguma intervenção, como uma reinicialização do serviço.

-   Como parte da clonagem, o novo DC Obtém uma nova identidade de computador e se provisiona como um DC de réplica na topologia. Valide se o aplicativo depende da identidade do computador, como seu nome, conta, SID e assim por diante. Ele se adapta automaticamente à alteração da identidade do computador no clone? Se esse aplicativo armazenar dados em cache, verifique se ele não depende dos dados de identidade do computador que podem ser armazenados em cache.

## <a name="what-is-interesting-for-application-vendors"></a>O que é interessante para fornecedores de aplicativos?

### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList. xml
Um controlador de domínio que executa seu aplicativo ou serviço não pode ser clonado até que o aplicativo ou serviço seja:

-   Adicionado ao arquivo CustomDCCloneAllowList. xml usando o cmdlet Get-ADDCCloningExcludedApplicationList do Windows PowerShell

-Or-

-   Removido do controlador de domínio

Na primeira vez que o usuário executa o cmdlet Get-ADDCCloningExcludedApplicationList, ele retorna uma lista de serviços e aplicativos que estão em execução no controlador de domínio, mas que não estão na lista padrão de serviços e aplicativos com suporte para clonagem. Por padrão, seu serviço ou aplicativo não será listado. Para adicionar seu serviço ou aplicativo à lista de aplicativos e serviços que podem ser clonados com segurança, o usuário executa o cmdlet Get-ADDCCloningExcludedApplicationList novamente com a opção-GenerateXML para adicioná-lo ao arquivo CustomDCCloneAllowList. xml. Para obter mais informações, consulte [etapa 2: executar o cmdlet Get-ADDCCloningExcludedApplicationList](https://docs.microsoft.com/powershell/module/addsadministration/get-addccloningexcludedapplicationlist).

### <a name="distributed-system-interactions"></a>Interações do sistema distribuído
Normalmente, os serviços isolados no computador local são aprovados ou reprovados quando participam da clonagem. Os serviços distribuídos precisam se preocupar em ter duas instâncias do computador host na rede simultaneamente por um breve período de tempo. Isso pode ser manifestado como uma instância de serviço que tenta efetuar pull de informações de um sistema de parceiros em que o clone se registrou como o novo fornecedor da identidade. Ou as duas instâncias do serviço podem enviar informações por push para o banco de dados AD DS ao mesmo tempo com resultados diferentes. Por exemplo, não é determinístico para qual computador será comunicado quando dois computadores que têm o serviço WTT (tecnologias de teste do Windows) estão na rede com o controlador de domínio.

Para o serviço do servidor DNS distribuído, o processo de clonagem evita cuidadosamente substituir os registros DNS do controlador de domínio de origem quando o controlador de domínio de clone começa com um novo endereço IP.

Você não deve confiar no computador para remover toda a identidade antiga até o fim da clonagem. Depois que o novo controlador de domínio for promovido dentro do novo contexto, selecione provedores de Sysprep são executados para limpar o estado adicional do computador. Por exemplo, nesse ponto, os certificados antigos do computador são removidos e os segredos de criptografia que o computador pode acessar são alterados.

O fator mais alto que varia o tempo da clonagem é quantos objetos existem para replicar do PDC. A mídia mais antiga aumenta o tempo necessário para concluir a clonagem.

Como seu serviço ou aplicativo é desconhecido, ele é deixado em execução. O processo de clonagem não altera o estado dos serviços que não são do Windows.

Além disso, o novo computador tem um endereço IP diferente do computador original. Esses comportamentos podem causar efeitos colaterais para seu serviço ou aplicativo, dependendo de como o serviço ou aplicativo se comporta nesse ambiente.

## <a name="additional-scenarios-suggested-for-testing"></a>Cenários adicionais sugeridos para teste

### <a name="cloning-failure"></a>Falha na clonagem
Os fornecedores de serviço devem testar esse cenário porque, quando a clonagem falha, o computador é inicializado no DSRM (modo de reparo de serviços de diretório), uma forma de modo de segurança. Neste momento, o computador não concluiu a clonagem. Algum estado pode ter sido alterado e algum estado pode permanecer do controlador de domínio original. Teste este cenário para entender o impacto que ele pode ter em seu aplicativo.

Para induzir uma falha de clonagem, tente clonar um controlador de domínio sem conceder permissão a ele para ser clonado. Nesse caso, o computador só terá alterado os endereços IP e ainda terá a maior parte de seu estado do controlador de domínio original. Para obter mais informações sobre como conceder uma permissão de controlador de domínio a ser clonada, consulte [etapa 1: conceder ao controlador de domínio virtualizado de origem a permissão a ser clonada](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/virtualized-domain-controller-deployment-and-configuration).

### <a name="pdc-emulator-cloning"></a>Clonagem de emulador de PDC
Os fornecedores de aplicativos e serviços devem testar esse cenário porque há uma reinicialização adicional quando o emulador de PDC é clonado. Além disso, a maior parte da clonagem é executada sob uma identidade temporária para permitir que o novo clone interaja com o emulador de PDC durante o processo de clonagem.

### <a name="writable-versus-read-only-domain-controllers"></a>Controladores de domínio graváveis versus somente leitura
Os fornecedores de aplicativos e serviços devem testar a clonagem usando o mesmo tipo de controlador de domínio (ou seja, em um controlador de domínio gravável ou somente leitura) no qual o serviço está planejado para ser executado.
