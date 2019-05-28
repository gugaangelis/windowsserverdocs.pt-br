---
title: Ativação automática de máquina virtual
TOCTitle: Automatic VM Activation
description: Como ativar VMs no Windows Server 2012 R2, Windows Server 2016 e Windows Server 2019
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 18e20433050371dc02782fb8630a885e53ae31ad
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63688700"
---
# <a name="automatic-virtual-machine-activation"></a>Ativação automática de máquina virtual

> Aplica-se a: Windows Server 2019, canal semestral do Windows Server, Windows Server 2016, Windows Server 2012 R2

AVMA (Ativação Automática da Máquina Virtual) funciona como um mecanismo de comprovante de compra, ajudando a garantir que os produtos Windows sejam usados de acordo com os Direitos de Uso do Produto e os Termos de Licença de Software da Microsoft.

A AVMA permite que você instale máquinas virtuais em um servidor Windows ativado corretamente sem ter que gerenciar chaves de produto para cada máquina virtual individual, mesmo em ambientes desconectados. A AVMA vincula a ativação da máquina virtual ao servidor de virtualização licenciado e ativa a máquina virtual quando ele é iniciado. A AVMA também fornece relatórios em tempo real sobre o uso e dados históricos sobre o estado da licença da máquina virtual. A emissão de relatórios e o controle de dados estão disponíveis no servidor de virtualização.

## <a name="practical-applications"></a>Aplicações práticas

Em servidores de virtualização que são ativados usando o Licenciamento por Volume ou o licenciamento do OEM, a AVMA oferece vários benefícios.

Os gerentes do datacenter do servidor podem usar a AVMA para fazer o seguinte:

  - Ativar máquinas virtuais em locais remotos

  - Ativar máquinas virtuais com ou sem uma conexão à Internet

  - Controlar o uso e as licenças da máquina virtual a partir do servidor de virtualização, sem exigir nenhum direito de acesso nos sistemas virtuais

Não há nenhuma chave de produto para gerenciar e nenhum adesivo para ler nos servidores. A máquina virtual é ativada e continua a funcionar mesmo quando é migrada em uma matriz de servidores de virtualização.

Os parceiros de SPLA (Contrato de Licença do Provedor de Serviços) e outros provedores de hospedagem não precisam compartilhar as chaves de produto com locatários ou acessar a máquina virtual de um locatário para ativá-la. A ativação da máquina virtual é transparente para o locatário quando a AVMA é utilizada. Os provedores de hospedagem podem usar os logs do servidor para verificar a conformidade da licença e acompanhar o histórico de uso do cliente.

## <a name="system-requirements"></a>Requisitos de sistema

A AVMA requer um servidor de virtualização da Microsoft executando o Windows Server 2012 R2, Windows Server 2016 Datacenter ou Windows Server Datacenter de 2019. 

Aqui estão os convidados que os hosts de versão diferentes podem ativar:

|Versão do servidor host|Windows Server 2019|Windows Server 2016|Windows Server 2012 R2|
|-|-|-|-|
|Windows Server 2019|X|X|X|
|Windows Server 2016| |X|X|
|Windows Server 2012 R2| ||X|

Observe que esses ativar todas as edições (Datacenter, Standard ou Essentials).

Essa ferramenta não funciona com outras tecnologias de virtualização de servidor.

## <a name="how-to-implement-avma"></a>Como implementar a AVMA

1.  Em um servidor de virtualização do Windows Server Datacenter, instale e configure a função de servidor do Microsoft Hyper-V. Para obter mais informações, consulte [instalar o Hyper-V Server](../virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server.md).

2.  [Criar uma máquina virtual](../virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v.md) e instalar um sistema operacional com suporte nele.

3.  Instale a chave AVMA na máquina virtual. Em um prompt de comando elevado, execute o seguinte comando:
    
    ``` 
    slmgr /ipk <AVMA_key>  
    ```

A máquina virtual ativará automaticamente a licença com relação ao servidor de virtualização.


> [!TIP]
> Você também pode usar as chaves AVMA em qualquer arquivo de configuração Unattend.exe.


## <a name="avma-keys"></a>Chaves AVMA

As chaves AVMA a seguir podem ser usadas para o Windows Server 2019.

|Edição|   Chave AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2D623-4GF74|
|Essentials|    2CTP7-NHT64-BP62M-FV6GG-HFV28|
 
As chaves AVMA a seguir podem ser usadas para o Windows Server, versão 1809.

|Edição|   Chave AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2D623-4GF74|

As chaves AVMA a seguir podem ser usadas para o Windows Server, versão 1803 e 1709.

|Edição|Chave AVMA|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|


As chaves AVMA a seguir podem ser usadas para o Windows Server 2016.

|Edição|Chave AVMA|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|
|Essentials|B4YNW-62DX9-W8V6M-82649-MHBKQ|


As chaves AVMA a seguir podem ser usadas para o Windows Server 2012 R2.

|Edição|Chave AVMA|
|-|-|
|Datacenter|Y4TGP-NPTV9-HTC2H-7MGQ3-DV4TW|
|Standard|DBGBW-NPF86-BJVTX-K3WKJ-MTB6V|
|Essentials|K2XGM-NMBT3-2R6Q8-WF2FK-P36R2|

## <a name="reporting-and-tracking"></a>Emissão de relatórios e acompanhamento

O Registro (KVP) no servidor de virtualização fornece dados de rastreamento em tempo real para os sistemas operacionais convidados. Como a chave de Registro é movida com a máquina virtual, você também pode obter informações sobre a licença. Por padrão, o KVP retorna informações sobre a máquina virtual, incluindo o seguinte:

  - Nome de domínio totalmente qualificado

  - Sistema operacional e service packs instalados

  - Arquitetura do processador

  - Endereços de rede IPv4 e IPv6

  - Endereços de RDP

Para obter mais informações sobre como obter essas informações, consulte [Script do Hyper-V: Procurando KVP guestintrinsicexchangeitems](http://blogs.msdn.com/b/virtual_pc_guy/archive/2008/11/18/hyper-v-script-looking-at-kvp-guestintrinsicexchangeitems.aspx).


> [!NOTE]
> Os dados de KPV não estão protegidos. Eles podem ser modificados e não são monitorados quanto a alterações.



> [!IMPORTANT]
> Os dados de KVP deverão ser removidos se a chave AVMA for substituída por outra chave de produto (chave de licenciamento por volume, comercial ou OEM).


Dados históricos sobre as solicitações da AVMA estão disponíveis em um arquivo de log no servidor de virtualização (EventID 12310).

Como o processo de ativação da AVMA é transparente, não são exibidas mensagens de erro. No entanto, os seguintes eventos são capturados em um arquivo de log nas máquinas virtuais (EventID 12309).

|Notificação|Descrição|
|-|-|
|Sucesso da AVMA|A máquina virtual foi ativada.|
|Host inválido|O servidor de virtualização não está respondendo. Isso pode acontecer quando o servidor não está executando uma versão suportada do Windows.|
|Dados inválidos|Isso geralmente é resultante de uma falha na comunicação entre o servidor de virtualização e a máquina virtual, geralmente causada por corrupção, criptografia ou incompatibilidade de dados.|
|Ativação Negada|O servidor de virtualização poderá não ativar o sistema operacional convidado porque a ID da AVMA não foi correspondente.|

