---
title: Instalação e upgrade do Windows Server
description: Como instalar, atualizar ou migrar para uma versão mais recente do Windows Server.
ms.prod: windows-server
ms.date: 05/14/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 98f876bd-63ff-4c3a-95d4-a8dd8d0d119c
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: f859253188c46d3e34e7a6ae504bf3eeafbae75c
ms.sourcegitcommit: 75f257d97d345da388cda972ccce0eb29e82d3bc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65613178"
---
# <a name="windows-server-installation-and-upgrade"></a>Atualização e instalação do Windows Server

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Windows Server 2019 está procurando? Ver [instalar, atualizar ou migrar para o Windows Server 2019](../get-started-19/install-upgrade-migrate-19.md).

> [!IMPORTANT]
> Suporte estendido para Windows Server 2008 R2 e Windows Server 2008 termina em janeiro de 2020. [Saiba mais sobre as opções de atualização](#upgrading-from-windows-server-2008-r2-or-windows-server-2008).

É hora de mudar para uma versão mais recente do Windows Server? Dependendo do que está executando agora, você tem muitas opções para chegar lá.

## <a name="installation"></a>Instalação

Se você deseja mudar para uma versão mais recente do Windows Server no mesmo hardware, uma maneira que funciona sempre é uma **instalação limpa**, onde basta instalar o sistema operacional mais recente diretamente sobre o antigo no mesmo hardware, excluindo, portanto, o sistema operacional anterior. Essa é a maneira mais simples, mas você precisará fazer backup de seus dados primeiro e se preparar para reinstalar seus aplicativos. Há algumas coisas que você deve saber, como os requisitos do sistema. Assim, verifique os detalhes do [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), do [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) e do [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

Mudar de qualquer versão de pré-lançamento (como o Windows Server 2016 Technical Preview) para a versão lançada (Windows Server 2016) sempre requer uma instalação limpa.

## <a name="migration-recommended-for-windows-server-2016"></a>Migração (recomendada para o Windows Server 2016)

Documentação de migração do Windows Server ajuda você a migrar de uma função ou recurso em vez de um computador de origem que executa o Windows Server para outro computador de destino que executa o Windows Server, a mesma ou uma versão mais recente. Para essas finalidades, a migração é definida como mover uma função ou um recurso e seus dados para um computador diferente, não atualizar o recurso no mesmo computador. Esse é o modo recomendado para mover seus dados e a carga de trabalho existente para uma versão mais recente do Windows Server. Para começar, verifique as [matriz de atualização e migração da função de servidor](https://go.microsoft.com/fwlink/?LinkId=828595) para o Windows Server.

## <a name="cluster-os-rolling-upgrade"></a>Atualização sem interrupção do sistema operacional do cluster
Atualização sem interrupção do sistema operacional do cluster é um novo recurso no Windows Server 2016 que permite ao administrador atualizar o sistema operacional dos nós do cluster do Windows Server 2012 R2 para o Windows Server 2016 sem interromper o Hyper-V ou as cargas de trabalho de Servidor de Arquivos de Escalabilidade Horizontal. Esse recurso permite que você evite o tempo de inatividade que poderia afetar os Contratos de nível de serviço. Esse recurso novo é abordado com mais detalhes em [Atualização sem interrupção do sistema de operacional do cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="license-conversion"></a>Conversão de licença
Em algumas versões do sistema operacional, é possível converter uma edição específica da versão para outra edição da mesma versão em uma única etapa, com um simples comando e com a chave de licença adequada. Isso é chamado de **conversão de licença**. Por exemplo, se seu servidor estiver executando o Windows Server 2016 Standard, será possível convertê-lo para o Windows Server 2016 Datacenter. Em algumas versões do Windows Server, você também pode converter livremente entre as versões OEM, com licença de volume e de varejo com o mesmo comando e a chave apropriada.

## <a name="upgrade"></a>Atualizar, Atualização (Upgrade)
Se você quiser manter o mesmo hardware e todas as funções de servidor que configurou sem planificar o servidor, a **atualização** é uma opção, e há muitas maneiras de fazer isso. Na atualização clássica, você vai de um sistema operacional antigo para um mais recente, mantendo suas configurações, funções de servidor e dados intactos. Por exemplo, se seu servidor estiver executando o Windows Server 2012 R2, será possível atualizá-lo para o Windows Server 2016. No entanto, nem todos os sistemas operacionais mais antigos têm um caminho para versões mais recentes.
 
>[!NOTE]
>A atualização funciona melhor em máquinas virtuais, onde os drivers de hardware específicos do OEM não são necessários para uma atualização bem-sucedida.
 
É possível atualizar de uma versão de avaliação do sistema operacional para uma versão comercial, de uma versão comercial mais antiga para uma versão mais nova ou, em alguns casos, de uma edição com licença de volume do sistema operacional para uma edição comercial comum.

Antes de começar com uma atualização, confira as tabelas nesta página para ver como é o processo de onde você está para onde quer chegar.

Para obter informações sobre as diferenças entre as opções de instalação disponíveis para o Windows Server 2016 Technical Preview, incluindo os recursos que são instalados com cada opção e as opões de gerenciamento disponíveis após a instalação, consulte [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828598).

>[!NOTE]
>Sempre que você migra ou atualiza para qualquer versão do Windows Server, deve consultar e entender a [política de ciclo de vida de suporte](https://support.microsoft.com/lifecycle) e o período para essa versão e planejar corretamente. Você pode [procurar as informações de ciclo de vida](https://support.microsoft.com/lifecycle) para a versão específica do Windows Server em que está interessado.
 
 
## <a name="upgrading-to-windows-server-2016"></a>Atualização para o Windows Server 2016
Para obter detalhes, incluindo restrições importantes e limitações durante a atualização, a conversão de licença entre as edições do Windows Server 2016 e a conversão das edições de avaliação para varejo, consulte [Caminhos de atualização permitidos para o Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828602).
 
>[!NOTE]
>Observação: Não há suporte para atualizações que alternam da instalação Server Core para uma instalação de Servidor com Desktop (e vice-versa). Se o sistema operacional mais antigo que está sendo atualizado ou convertido for uma instalação Server Core, o resultado ainda será uma instalação Server Core do sistema operacional mais recente.
 
Tabela de referência rápida dos caminhos de atualização permitidos de edições de varejo mais antigas do Windows Server para edições de varejo do Windows Server 2016:


|Se você está executando estas versões e edições:|É possível atualizar para estas versões e edições:|
|--------------------------------|---------------------------------------|
|Windows Server 2012 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard ou Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Hyper-V Server 2012 R2|Hyper-V Server 2016 (usando o recurso de atualização sem interrupção do sistema operacional do cluster)|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Grupo de trabalho do Windows Storage Server 2012|Grupo de trabalho do Windows Storage Server 2016|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Grupo de trabalho do Windows Storage Server 2016|
 
### <a name="license-conversion"></a>Conversão de licença
É possível converter o Windows Server 2016 Standard (varejo) no Windows Server 2016 Datacenter (varejo).

É possível converter o Windows Server 2016 Essentials (varejo) no Windows Server 2016 Standard (varejo).

É possível converter a versão de avaliação do Windows Server 2016 Standard para o Windows Server 2016 Standard (comercial) ou para o Datacenter (comercial).

É possível converter a versão de avaliação do Windows Server 2016 Datacenter no Windows Server 2016 Datacenter (varejo).
 
## <a name="upgrading-to-windows-server-2012-r2"></a>Atualização para o Windows Server 2012 R2
Para obter detalhes, incluindo restrições importantes e limitações durante a atualização, a conversão de licença entre as edições do Windows Server 2012 R2 e a conversão das edições de avaliação para varejo, consulte [Opções de atualização para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn303416.aspx).

Tabela de referência rápida dos caminhos de atualização permitidos de edições de varejo mais antigas do Windows Server para edições de varejo do Windows Server 2012 R2:

|Se você está executando:|É possível atualizar para estas edições:|
|-------------------------|---------------------------|
|Windows Server 2008 R2 Datacenter com SP1|Windows Server 2012 R2 Datacenter|
|Windows Server 2008 R2 Enterprise com SP1|Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter|
|Windows Server 2008 R2 Standard com SP1|Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter|
|Windows Web Server 2008 R2 com SP1|Windows Server 2012 R2 Standard|
|Windows Server 2012 Datacenter|Windows Server 2012 R2 Datacenter|
|Windows Server 2012 Standard|Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter|
|Hyper-V Server 2012|Hyper-V Server 2012 R2|

### <a name="license-conversion"></a>Conversão de licença
É possível converter o Windows Server 2012 Standard (varejo) no Windows Server 2012 Datacenter (varejo).

É possível converter o Windows Server 2012 Essentials (varejo) no Windows Server 2012 Standard (varejo).

É possível converter a versão de avaliação do Windows Server 2012 Standard para o Windows Server 2012 Standard (comercial) ou para o Datacenter (comercial).

## <a name="upgrading-to-windows-server-2012"></a>Atualização para o Windows Server 2012
Para obter detalhes, incluindo restrições importantes e limitações sobre atualização e conversão das edições de avaliação para varejo, consulte [Versões de avaliação e opções de atualização para o Windows Server 2012](https://technet.microsoft.com/library/jj574204.aspx).
 
Tabela de referência rápida dos caminhos de atualização permitidos de edições de varejo mais antigas do Windows Server para edições de varejo do Windows Server 2012:

|Se você está executando:|É possível atualizar para estas edições:|
|--------------------------|--------------------------|
|Windows Server 2008 Standard com SP2 ou Windows Server 2008 Enterprise com SP2|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 Datacenter com SP2|Windows Server 2012 Datacenter|
|Windows Web Server 2008|Windows Server 2012 Standard|
|Windows Server 2008 R2 Standard com SP1 ou Windows Server Enterprise 2008 R2 com SP1|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 R2 Datacenter com SP1|Windows Server 2012 Datacenter|
|Windows Web Server 2008 R2|Windows Server 2012 Standard|

### <a name="license-conversion"></a>Conversão de licença
É possível converter o Windows Server 2012 Standard (varejo) no Windows Server 2012 Datacenter (varejo).

É possível converter o Windows Server 2012 Essentials (varejo) no Windows Server 2012 Standard (varejo).

É possível converter a versão de avaliação do Windows Server 2012 Standard para o Windows Server 2012 Standard (comercial) ou para o Datacenter (comercial).

## <a name="upgrading-from-windows-server-2008-r2-or-windows-server-2008"></a>Atualização do Windows Server 2008 R2 ou Windows Server 2008

Conforme descrito em [atualizar o Windows Server 2008 e Windows Server 2008 R2](modernize-windows-server-2008.md), o suporte estendido para o Windows Server 2008 R2 de Windows Server 2008 termina em janeiro de 2020. Para garantir que nenhum intervalo no suporte, você precisará atualizar para uma versão com suporte do Windows Server ou hospedar novamente no Azure, movendo para [especializada em VMs do Windows Server 2008 R2](uploading-specialized-WS08-image-to-azure.md). Confira a [guia de migração para o Windows Server](https://go.microsoft.com/fwlink/?linkid=872689) para obter informações e considerações para planejar a migração/atualização.

Para servidores locais, não há nenhum caminho direto de atualização do Windows Server 2008 R2 para o Windows Server 2016 ou posterior. Em vez disso, atualize primeiro para o Windows Server 2012 R2 e, em seguida [atualizar para o Windows Server 2016](#upgrading-to-windows-server-2016).

Como planejar sua atualização, esteja ciente das diretrizes a seguir para a etapa intermediária de atualização para o Windows Server 2012 R2.

  - Você não pode fazer uma atualização in-loco de um arquiteturas de 32 bits para 64 bits ou de um tipo para outro (fre para chk, por exemplo) de compilação.

  - Somente há suporte para atualizações in-loco no mesmo idioma. Você não pode atualizar de um idioma para outro.

  - Você não pode migrar de uma instalação server core do Windows Server 2008 para o Windows Server 2012 R2 com a GUI do servidor (chamado de "Servidor com área de trabalho completa" no Windows Server). Você pode alternar sua instalação de núcleo do servidor atualizado para o servidor com a área de trabalho completa, mas apenas no Windows Server 2012 R2. Windows Server 2016 e posterior *não* oferecem suporte a troca do server core para área de trabalho completa, portanto, essa opção antes de atualizar para o Windows Server 2016.
  
Para obter mais informações, confira [versões de avaliação e opções de atualização para o Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574204\(v=ws.11\)), que inclui detalhes de atualização de função específica.

