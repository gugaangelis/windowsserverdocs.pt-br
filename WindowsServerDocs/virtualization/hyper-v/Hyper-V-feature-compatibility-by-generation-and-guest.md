---
title: Compatibilidade de recursos do Hyper-V por geração e convidado
description: Lista as gerações e sistemas operacionais que são compatíveis com os principais recursos do Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 81c1f32d-7814-4992-8a66-dd4b77c939b4
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 5950a75da4569979794a5848bd41ab349dc34676
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812659"
---
# <a name="hyper-v-feature-compatibility-by-generation-and-guest"></a>Compatibilidade de recursos do Hyper-V por geração e convidado

>Aplica-se a: Windows Server 2016
  
As tabelas neste artigo mostram as gerações e sistemas operacionais que são compatíveis com alguns dos recursos do Hyper-V, agrupados por categorias. Em geral, você obterá a melhor disponibilidade de recursos com uma máquina virtual de geração 2 que executa o sistema operacional mais recente.  
  
Tenha em mente que alguns recursos dependem de hardware ou outra infraestrutura. Para obter detalhes de hardware, consulte [requisitos de sistema do Hyper-V no Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). Em alguns casos, um recurso pode ser usado com qualquer sistema operacional convidado com suporte. Para obter detalhes sobre quais sistemas operacionais são suportados, consulte:  
  
* [Máquinas virtuais Linux e FreeBSD com suporte](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
* [Sistemas operacionais convidados compatíveis com o Windows](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)  
  
## <a name="availability-and-backup"></a>Disponibilidade e backup  
  
Recurso  | geração | Sistema operacional convidado  
------------- | ------------- | -----------  
Pontos de verificação | 1 e 2 | Qualquer convidado com suporte  
Clustering de convidado | 1 e 2 | Convidados que executam aplicativos com reconhecimento de cluster e têm o software de destino iSCSI instalado  
Replicação | 1 e 2 | Qualquer convidado com suporte  
Controlador de domínio | 1 e 2 | Qualquer suporte convidado do Windows Server usando somente pontos de verificação de produção. Consulte [sistemas de operacionais de convidados com suporte do Windows Server](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows#supported-windows-server-guest-operating-systems)   
  
## <a name="compute"></a>Computação  
  
Recurso  | geração | Sistema operacional convidado  
------------- | ------------- | -----------  
Memória dinâmica | 1 e 2 | Versões específicas de convidados com suporte. Ver [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx) para versões anteriores ao Windows Server 2016 e Windows 10.  
Adicionar/remoção dinâmicas de memória | 1 e 2 | Windows Server 2016, Windows 10  
NUMA Virtual | 1 e 2 | Qualquer convidado com suporte  
  
## <a name="development-and-test"></a>Desenvolvimento e teste  
Recurso  | geração | Sistema operacional convidado  
------------- | ------------- | -----------  
Portas seriais/COM | 1 e 2 <br>**Observação:** Para a geração 2, use o Windows PowerShell para configurar. Para obter detalhes, consulte [adicionar uma porta COM para depuração de kernel](./plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md#add-a-com-port-for-kernel-debugging). | Qualquer convidado com suporte  
  
## <a name="mobility"></a>Mobilidade  
  
Recurso  | geração | Sistema operacional convidado  
------------- | ------------- | -----------  
Migração ao vivo  | 1 e 2 |  Qualquer convidado com suporte  
Importação/exportação | 1 e 2 |  Qualquer convidado com suporte  
  
## <a name="networking"></a>Rede  
  
Recurso  | geração | Sistema operacional convidado  
------------- | ------------- | -----------  
Adicionar/remoção dinâmicas de adaptador de rede virtual | 2 | Qualquer convidado com suporte  
Adaptador de rede virtual herdado | 1 | Qualquer convidado com suporte  
Virtualização de entrada/saída de raiz única (SR-IOV) | 1 e 2 | convidados do Windows 64 bits, começando com o Windows Server 2012 e Windows 8.  
Fila de várias máquinas virtuais (VMMQ) | 1 e 2  | Qualquer convidado com suporte  
  
## <a name="remote-connection-experience"></a>Experiência de conexão remota  
  
Recurso  | geração | Sistema operacional convidado  
------------- | ------------- | -----------  
Atribuição de dispositivo discretos (DDA) | 1 e 2 | Windows Server 2016, Windows Server 2012 R2 somente com a atualização 3133690 instalado, o Windows 10 <br> **Observação:** Para obter detalhes sobre 3133690 de atualização, consulte [isso](https://support.microsoft.com/kb/3133690) artigo de suporte.  
Modo de sessão avançado | 1 e 2 | Windows Server 2016, Windows Server 2012 R2, Windows 10 e Windows 8.1, com os serviços de área de trabalho remota habilitada <br>**Observação**: Talvez seja necessário também configurar o host. Para obter detalhes, consulte [usar recursos locais na máquina virtual Hyper-V com VMConnect](./learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).  
RemoteFx | 1 e 2 | Geração 1 nas versões Windows 32 bits e 64 bits, começando com o Windows 8. <br> Geração 2 em versões de 64 bits do Windows 10  
  
## <a name="security"></a>Segurança  
  
Recurso  | geração | Sistema operacional convidado  
------------- | ------------- | -----------  
Inicialização segura | 2 | **Linux**: Ubuntu 14.04 e versões posterior, SUSE Linux Enterprise Server 12 e posterior, o Red Hat Enterprise Linux 7.0 e posterior e CentOS 7.0 e versões posteriores<br>**Windows**: Todas as versões suportadas que podem ser executados em uma máquina virtual de geração 2  
Máquinas virtuais blindadas | 2 | **Windows**: Todas as versões suportadas que podem ser executados em uma máquina virtual de geração 2  
  
## <a name="storage"></a>Armazenamento  
  
Recurso  | geração | Sistema operacional convidado  
------------- | ------------- | -----------  
Discos de rígidos virtuais compartilhados (VHDX) | 1 e 2  | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012  
SMB3 | 1 e 2 | Todos os que dão suporte a SMB3  
Espaços de armazenamento diretos | 2 | Windows Server 2016  
Fibre Channel Virtual | 1 e 2 | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012  
Formato VHDX | 1 e 2 | Qualquer convidado com suporte   
  
  
  
  
    


