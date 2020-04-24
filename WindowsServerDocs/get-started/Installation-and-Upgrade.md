---
title: Instalação e atualização do Windows Server
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
ms.openlocfilehash: 140f67a9dab5cf1f10cdb0c5c51a031a0dfb9dd3
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "79323638"
---
# <a name="windows-server-installation-and-upgrade"></a>Instalação e atualização do Windows Server

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Procurando pelo Windows Server 2019? Confira [Instalar, atualizar ou migrar para o Windows Server 2019](../get-started-19/install-upgrade-migrate-19.md).

> [!IMPORTANT]
> O suporte estendido para Windows Server 2008 R2 e Windows Server 2008 termina em janeiro de 2020. [Saiba mais sobre as opções de atualização](#upgrading-from-windows-server-2008-r2-or-windows-server-2008).

É hora de mudar para uma versão mais recente do Windows Server? Dependendo do que estiver executando agora, você tem muitas opções para chegar lá.

## <a name="installation"></a>Instalação

Se você deseja mudar para uma versão mais recente do Windows Server no mesmo hardware, uma maneira que sempre funciona é uma **instalação limpa**, em que basta instalar o sistema operacional mais recente diretamente sobre o antigo no mesmo hardware, excluindo assim o sistema operacional anterior. Essa é a maneira mais simples, mas primeiro é necessário fazer backup de seus dados e se preparar para reinstalar seus aplicativos. Há algumas coisas que você deve saber, como os requisitos do sistema, então, verifique os detalhes do [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), do [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) e do [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

Mudar de qualquer versão de pré-lançamento (como o Windows Server 2016 Technical Preview) para a versão lançada (Windows Server 2016) sempre requer uma instalação limpa.

## <a name="migration-recommended-for-windows-server-2016"></a>Migração (recomendada para o Windows Server 2016)

A documentação de migração do Windows Server ajuda você a migrar uma função ou um recurso de cada vez de um computador de origem que está executando o Windows Server para outro computador de destino que está executando o Windows Server, com uma versão igual ou mais recente. Para essas finalidades, a migração é definida como mover uma função ou um recurso e seus dados para um computador diferente, e não como atualizar o recurso no mesmo computador. Esse é o modo recomendado para mover seus dados e a carga de trabalho existente para uma versão mais recente do Windows Server. Para começar, verifique a [matriz de atualização e migração da função de servidor](https://go.microsoft.com/fwlink/?LinkId=828595) para Windows Server.

## <a name="cluster-os-rolling-upgrade"></a>Atualização sem interrupção do sistema operacional do cluster
A Atualização sem interrupção do sistema operacional do cluster é um novo recurso no Windows Server 2016 que permite ao administrador atualizar o sistema operacional dos nós do cluster do Windows Server 2012 R2 para o Windows Server 2016 sem interromper o Hyper-V ou as cargas de trabalho de Servidor de Arquivos de Escalabilidade Horizontal. Esse recurso permite que você evite o tempo de inatividade que poderia afetar os Contratos de nível de serviço. Esse recurso novo é abordado com mais detalhes em [Atualização sem interrupção do sistema de operacional do cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="license-conversion"></a>Conversão de licença
Em algumas versões do sistema operacional, é possível converter uma edição específica da edição para outra edição da mesma versão em uma única etapa, usando um simples comando e com a chave de licença adequada. Isso é chamado de **conversão de licença**. Por exemplo, se o servidor estiver executando o Windows Server 2016 Standard, é possível convertê-lo para o Windows Server 2016 Datacenter. Em algumas versões do Windows Server, também é possível fazer conversões livremente entre as versões OEM, de licença por volume e de varejo com o mesmo comando e a chave apropriada.

## <a name="upgrade"></a>Atualizar, Atualização (Upgrade)
Se você quer manter o mesmo hardware e todas as funções de servidor que configurou sem fazer uma nova instalação, a **atualização** é uma opção, e há muitas maneiras de fazer isso. Na atualização clássica, você vai de um sistema operacional antigo para um mais recente, mantendo suas configurações, funções de servidor e dados intactos. Por exemplo, se seu servidor executa o Windows Server 2012 R2, é possível atualizá-lo para o Windows Server 2016. Entretanto, nem todos os sistemas operacionais mais antigos têm um caminho para versões mais recentes.
 
>[!NOTE]
>A atualização funciona melhor em máquinas virtuais, onde os drivers de hardware específicos do OEM não são necessários para uma atualização bem-sucedida.
 
É possível atualizar de uma versão de avaliação do sistema operacional para uma versão comercial, de uma versão comercial mais antiga para uma versão mais nova ou, em alguns casos, de uma edição com licença de volume do sistema operacional para uma edição comercial comum.

Antes de começar com uma atualização, confira as tabelas nesta página para ver como é o processo de onde você está até onde quer chegar.

Para saber mais sobre as diferenças entre as opções de instalação disponíveis para o Windows Server 2016 Technical Preview, incluindo os recursos que são instalados com cada opção e as opções de gerenciamento disponíveis após a instalação, confira [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828598).

>[!NOTE]
>Sempre que você migrar ou atualizar para qualquer versão do Windows Server, deve consultar e entender a [política de ciclo de vida de suporte](https://support.microsoft.com/lifecycle) e o período para essa versão, e planejar corretamente. Você pode [procurar as informações sobre o ciclo de vida](https://support.microsoft.com/lifecycle) para a versão específica do Windows Server em que está interessado.
 
 
## <a name="upgrading-to-windows-server-2016"></a>Atualização para o Windows Server 2016
Para saber mais, incluindo limitações e restrições importantes sobre a atualização, a conversão de licença entre as edições do Windows Server 2016 e a conversão das edições de avaliação para varejo, confira [Caminhos de atualização compatíveis para o Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828602).
 
>[!NOTE]
>Observação: Não há suporte para atualizações que alternam da instalação Server Core para uma instalação de Servidor com Desktop (e vice-versa). Se o sistema operacional mais antigo que está sendo atualizado ou convertido para uma instalação é o Server Core, o resultado ainda será uma instalação Server Core do sistema operacional mais recente.
 
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

É possível converter a versão de avaliação do Windows Server 2016 Datacenter em Windows Server 2016 Datacenter (varejo).
 
## <a name="upgrading-to-windows-server-2012-r2"></a>Atualização para o Windows Server 2012 R2
Para saber mais, incluindo limitações e restrições importantes sobre a atualização, a conversão de licença entre as edições do Windows Server 2012 R2 e a conversão das edições de avaliação para varejo, confira [Opções de atualização para o Windows Server 2012 R2](https://technet.microsoft.com/library/dn303416.aspx).

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
É possível converter o Windows Server 2012 Standard (varejo) em Windows Server 2012 Datacenter (varejo).

É possível converter o Windows Server 2012 Essentials (varejo) em Windows Server 2012 Standard (varejo).

É possível converter a versão de avaliação do Windows Server 2012 Standard em Windows Server 2012 Standard (comercial) ou Datacenter (comercial).

## <a name="upgrading-to-windows-server-2012"></a>Atualização para o Windows Server 2012
Para saber mais, incluindo limitações e restrições importantes sobre atualização e conversão das edições de avaliação para varejo, confira [Versões de avaliação e opções de atualização para o Windows Server 2012](https://technet.microsoft.com/library/jj574204.aspx).
 
Tabela de referência rápida dos caminhos de atualização permitidos de edições de varejo mais antigas do Windows Server para edições de varejo do Windows Server 2012:

|Se você está executando:|É possível atualizar para estas edições:|
|--------------------------|--------------------------|
|Windows Server 2008 Standard com SP2 ou Windows Server 2008 Enterprise com SP2|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 Datacenter com SP2|Windows Server 2012 Datacenter|
|Windows Web Server 2008|Windows Server 2012 Standard|
|Windows Server 2008 R2 Standard com SP1 ou Windows Server 2008 R2 Enterprise com SP1|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 R2 Datacenter com SP1|Windows Server 2012 Datacenter|
|Windows Web Server 2008 R2|Windows Server 2012 Standard|

### <a name="license-conversion"></a>Conversão de licença
É possível converter o Windows Server 2012 Standard (varejo) em Windows Server 2012 Datacenter (varejo).

É possível converter o Windows Server 2012 Essentials (varejo) em Windows Server 2012 Standard (varejo).

É possível converter a versão de avaliação do Windows Server 2012 Standard em Windows Server 2012 Standard (comercial) ou Datacenter (comercial).

## <a name="upgrading-from-windows-server-2008-r2-or-windows-server-2008"></a>Atualização do Windows Server 2008 R2 ou Windows Server 2008

Conforme descrito em [Atualizar o Windows Server 2008 e Windows Server 2008 R2](modernize-windows-server-2008.md), o suporte estendido para o Windows Server 2008 R2/Windows Server 2008 termina em janeiro de 2020. Para garantir que não haja intervalo no suporte, você precisará atualizar para uma versão compatível do Windows Server ou hospedar novamente no Azure, mudando para [as VMs especializadas do Windows Server 2008 R2](uploading-specialized-WS08-image-to-azure.md). Confira o [Guia de migração para o Windows Server](https://go.microsoft.com/fwlink/?linkid=872689) para saber mais e ver considerações para planejar a migração/atualização.

Para servidores locais, não há nenhum caminho direto de atualização do Windows Server 2008 R2 para o Windows Server 2016 ou posterior. Em vez disso, atualize primeiro para o Windows Server 2012 R2 e, em seguida, [atualize para o Windows Server 2016](#upgrading-to-windows-server-2016).

Ao planejar sua atualização, esteja ciente das diretrizes a seguir para a etapa intermediária da atualização para o Windows Server 2012 R2.

  - Você não pode fazer uma atualização in-loco de uma arquitetura de 32 bits para 64 bits, ou de um tipo de compilação para outro (fre para chk, por exemplo).

  - Há suporte somente para atualizações in-loco no mesmo idioma. Não é possível atualizar de um idioma para outro.

  - Você não pode migrar de uma instalação Server Core do Windows Server 2008 para o Windows Server 2012 R2 com a GUI do servidor (chamada de "Servidor com área de trabalho completa" no Windows Server). É possível alternar sua instalação de server core atualizada para o servidor com a Área de trabalho completa, mas apenas no Windows Server 2012 R2. O Windows Server 2016 e posteriores *não* dão suporte à troca do Server Core para Área de trabalho completa, portanto, realize essa mudança antes de atualizar para o Windows Server 2016.
  
Para saber mais, confira [Versões de avaliação e opções de atualização para o Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574204\(v=ws.11\)), que inclui detalhes sobre a atualização específica para funções.

