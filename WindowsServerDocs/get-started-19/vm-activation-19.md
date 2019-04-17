---
title: Ativação de máquina virtual automática
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
ms.openlocfilehash: 62873140c8e114ba537dc4fd3ff7c44868c33243
ms.sourcegitcommit: ca5c80bdb903b282e292488172a7cc92546574c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4375483"
---
# Ativação de máquina virtual automática

> Aplica-se a: Windows Server 2019, canal semestral do Windows Server, Windows Server 2016, Windows Server 2012 R2

Ativação de máquina Virtual automática (AVMA) atua como um mecanismo de comprovação de compra, ajudando a garantir que produtos Windows sejam usados de acordo com os direitos de uso de produto e os termos de licença de Software Microsoft.

AVMA permite que você instale as máquinas virtuais em um servidor de Windows corretamente ativado sem a necessidade de gerenciar chaves de produto para cada máquina virtual individual, mesmo em ambientes desconectados. AVMA associa a ativação de máquina virtual para o servidor de virtualização licenciados e ativa a máquina virtual quando ele é iniciado. AVMA também fornece relatórios em tempo real sobre o uso e dados históricos sobre o estado da licença da máquina virtual. Dados de rastreamento e relatório estão disponível no servidor de virtualização.

## Aplicações práticas

Em servidores de virtualização ativados com o licenciamento de licenciamento por Volume ou OEM, AVMA oferece vários benefícios.

Os gerentes de datacenter do servidor podem usar AVMA para fazer o seguinte:

  - Ativar as máquinas virtuais em locais remotos

  - Ativar as máquinas virtuais com ou sem uma conexão de internet

  - Controlar o uso de máquina virtual e licenças do servidor de virtualização, sem a necessidade de todos os direitos de acesso nos sistemas virtualizados

Há sem chaves de produto para gerenciar e nenhuma selos nos servidores de ler. A máquina virtual está ativada e continuará a funcionar mesmo quando ele é migrado em uma matriz de servidores de virtualização.

Parceiros do contrato de licença de provedor de serviço (SPLA) e outros provedores de hospedagem não precisam compartilhar chaves de produto com locatários ou acessar a VM do locatário para ativá-la. Ativação de máquina virtual é transparente para o locatário quando AVMA é usado. Provedores de hospedagem podem usar os logs do servidor para verificar a conformidade de licença e rastrear o histórico de uso do cliente.

## Requisitos de sistema

AVMA requer um servidor de virtualização da Microsoft que executam o Windows Server 2012 R2, Windows Server 2016 Datacenter ou Windows Server 2019 Datacenter. 

Aqui estão os convidados que podem ativar os hosts de versão diferentes:

|Versão do servidor host|Windows Server 2019|Windows Server 2016|Windows Server 2012 R2|
|-|-|-|-|
|Windows Server 2019|X|X|X|
|Windows Server 2016| |X|X|
|Windows Server 2012 R2| ||X|

Observe que essas ative todas as edições (Datacenter, Standard ou Essentials).

Essa ferramenta não funciona com outras tecnologias de virtualização de servidor.

## Como implementar AVMA

1.  Em um servidor de virtualização do Windows Server Datacenter, instalar e configurar a função de servidor do Microsoft Hyper-V. Para obter mais informações, consulte [Instalar o Hyper-V Server](../virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server.md).

2.  [Criar uma máquina virtual](../virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v.md) e instalar um sistema operacional de servidor com suporte nele.

3.  Instale a chave AVMA na máquina virtual. Em um prompt de comando com privilégios elevados, execute o seguinte comando:
    
    ``` 
    slmgr /ipk <AVMA_key>  
    ```

A máquina virtual será ativado automaticamente a licença em relação ao servidor de virtualização.


> [!TIP]
> Você também pode usar as chaves AVMA qualquer arquivo de instalação Unattend.exe.


## Chaves AVMA

As seguintes chaves do AVMA podem ser usadas para o Windows Server 2019.

|Edição|   Chave AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B - 2D 623-4GF74|
|Essenciais|    2CTP7-NHT64-BP62M-FV6GG-HFV28|
 
As seguintes chaves do AVMA podem ser usadas para o Windows Server, versão 1809.

|Edição|   Chave AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B - 2D 623-4GF74|

As seguintes chaves do AVMA podem ser usadas para o Windows Server, versão 1803 e 1709.

|Edição|Chave AVMA|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|


As seguintes chaves do AVMA podem ser usadas para o Windows Server 2016.

|Edição|Chave AVMA|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|
|Essenciais|B4YNW-62DX9-W8V6M-82649-MHBKQ|


As seguintes chaves do AVMA podem ser usadas para o Windows Server 2012 R2.

|Edição|Chave AVMA|
|-|-|
|Datacenter|Y4TGP-NPTV9-HTC2H-7MGQ3-DV4TW|
|Standard|DBGBW-NPF86-BJVTX-K3WKJ-MTB6V|
|Essenciais|K2XGM-NMBT3-2R6Q8-WF2FK-P36R2|

## Relatório e controle

O registro (KVP) no servidor de virtualização fornece dados de controle em tempo real para os sistemas operacionais convidados. Como a chave do registro se move com a máquina virtual, você pode obter informações de licença também. Por padrão o KVP retorna informações sobre a máquina virtual, incluindo o seguinte:

  - Nome de domínio totalmente qualificado

  - Sistema operacional e service packs instalados

  - Arquitetura do processador

  - Endereços de rede IPv4 e IPv6

  - Endereços RDP

Para obter mais informações sobre como obter essas informações, consulte [Script do Hyper-V: olhando KVP GuestIntrinsicExchangeItems](http://blogs.msdn.com/b/virtual_pc_guy/archive/2008/11/18/hyper-v-script-looking-at-kvp-guestintrinsicexchangeitems.aspx).


> [!NOTE]
> Dados KVP não estão protegidos. Ele pode ser modificado e as alterações não é monitorado.



> [!IMPORTANT]
> Dados KVP deverá ser removidos se a chave AVMA for substituída por outra chave do produto (varejo, OEM ou volume chave de licença).


Dados históricos sobre solicitações AVMA estão disponíveis em um arquivo de log no servidor de virtualização (EventID 12310).

Como o processo de ativação AVMA é transparente, mensagens de erro não são exibidas. No entanto, os seguintes eventos são capturados em um arquivo de log nas máquinas virtuais (EventID 12309).

|Notificação|Descrição|
|-|-|
|Sucesso AVMA|A máquina virtual foi ativada.|
|Host inválido|O servidor de virtualização não está respondendo. Isso pode acontecer quando o servidor não estiver executando uma versão com suporte do Windows.|
|Dados inválidos|Isso geralmente resulta de uma falha na comunicação entre o servidor de virtualização e a máquina virtual, geralmente causado por incompatibilidade de dados, criptografia ou corrupção.|
|Ativação negada|O servidor de virtualização não pode ativar o sistema operacional convidado porque a ID de AVMA não correspondia.|

