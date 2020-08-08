---
title: Compatibilidade de recursos do Hyper-V por geração e convidado
description: Lista as gerações e os sistemas operacionais que são compatíveis com os principais recursos do Hyper-V
manager: dongill
ms.topic: article
ms.assetid: 81c1f32d-7814-4992-8a66-dd4b77c939b4
author: kbdazure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 5de7f55d9fead7b720991749dd1c83aa727636c4
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997030"
---
# <a name="hyper-v-feature-compatibility-by-generation-and-guest"></a>Compatibilidade de recursos do Hyper-V por geração e convidado

>Aplica-se a: Windows Server 2016

As tabelas neste artigo mostram as gerações e os sistemas operacionais que são compatíveis com alguns dos recursos do Hyper-V, agrupados por categorias. Em geral, você obterá a melhor disponibilidade dos recursos com uma máquina virtual de geração 2 que executa o sistema operacional mais recente.

Tenha em mente que alguns recursos dependem de hardware ou de outra infraestrutura. Para obter detalhes sobre o hardware, consulte [requisitos do sistema para o Hyper-V no Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). Em alguns casos, um recurso pode ser usado com qualquer sistema operacional convidado com suporte. Para obter detalhes sobre quais sistemas operacionais têm suporte, consulte:

* [Máquinas virtuais Linux e FreeBSD com suporte](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
* [Sistemas operacionais convidados compatíveis com o Windows](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="availability-and-backup"></a>Disponibilidade e backup

Recurso  | Generation | Sistema operacional convidado
------------- | ------------- | -----------
Pontos de verificação | 1 e 2 | Qualquer convidado com suporte
Clustering de convidado | 1 e 2 | Convidados que executam aplicativos com reconhecimento de cluster e têm software de destino iSCSI instalado
Replicação | 1 e 2 | Qualquer convidado com suporte
Controlador de domínio | 1 e 2 | Qualquer convidado do Windows Server com suporte usando apenas pontos de verificação de produção. Consulte [sistemas operacionais convidados do Windows Server com suporte](./supported-windows-guest-operating-systems-for-hyper-v-on-windows.md#supported-windows-server-guest-operating-systems)

## <a name="compute"></a>Computação

Recurso  | Generation | Sistema operacional convidado
------------- | ------------- | -----------
Memória dinâmica | 1 e 2 | Versões específicas de convidados com suporte. Consulte [visão geral de memória dinâmica do Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831766(v=ws.11)) para versões anteriores ao windows Server 2016 e Windows 10.
Adição/remoção de memória a quente | 1 e 2 | Windows Server 2016, Windows 10
NUMA Virtual | 1 e 2 | Qualquer convidado com suporte

## <a name="development-and-test"></a>Desenvolvimento e teste
Recurso  | Generation | Sistema operacional convidado
------------- | ------------- | -----------
Portas seriais/COM | 1 e 2 <br>**Observação:** Para a geração 2, use o Windows PowerShell para configurar o. Para obter detalhes, consulte [Adicionar uma porta com para depuração de kernel](./plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md#add-a-com-port-for-kernel-debugging). | Qualquer convidado com suporte

## <a name="mobility"></a>Mobilidade

Recurso  | Generation | Sistema operacional convidado
------------- | ------------- | -----------
Migração ao vivo  | 1 e 2 |  Qualquer convidado com suporte
Importar/exportar | 1 e 2 |  Qualquer convidado com suporte

## <a name="networking"></a>Rede

Recurso  | Generation | Sistema operacional convidado
------------- | ------------- | -----------
Adição/remoção ativa do adaptador de rede virtual | 2 | Qualquer convidado com suporte
Adaptador de rede virtual herdado | 1 | Qualquer convidado com suporte
Virtualização de entrada/saída de raiz única (SR-IOV) | 1 e 2 | convidados do Windows de 64 bits, começando com o Windows Server 2012 e o Windows 8.
VMMQ (várias filas de máquina virtual) | 1 e 2  | Qualquer convidado com suporte

## <a name="remote-connection-experience"></a>Experiência de conexão remota

Recurso  | Generation | Sistema operacional convidado
------------- | ------------- | -----------
Atribuição de dispositivo discreta (DDA) | 1 e 2 | Windows Server 2016, Windows Server 2012 R2 somente com a atualização 3133690 instalada, Windows 10 <br> **Observação:** Para obter detalhes sobre a atualização 3133690, consulte [este](https://support.microsoft.com/kb/3133690) artigo de suporte.
Modo de sessão avançado | 1 e 2 | Windows Server 2016, Windows Server 2012 R2, Windows 10 e Windows 8.1, com Serviços de Área de Trabalho Remota habilitado <br>**Observação**: Talvez seja necessário configurar também o host. Para obter detalhes, consulte [usar recursos locais na máquina virtual do Hyper-V com VMConnect](./learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).
GPUs | 1 e 2 | Geração 1 em versões de 32 bits e 64 bits do Windows a partir do Windows 8. <br> Geração 2 em versões do Windows 10 de 64 bits

## <a name="security"></a>Segurança

Recurso  | Generation | Sistema operacional convidado
------------- | ------------- | -----------
Inicialização Segura | 2 | **Linux**: Ubuntu 14, 4 e posterior, SuSE Linux Enterprise Server 12 e posterior, Red Hat Enterprise Linux 7,0 e posterior e CentOS 7,0 e posterior<br>**Windows**: todas as versões com suporte que podem ser executadas em uma máquina virtual de geração 2
Máquinas virtuais blindadas | 2 | **Windows**: todas as versões com suporte que podem ser executadas em uma máquina virtual de geração 2

## <a name="storage"></a>Armazenamento

Recurso  | Generation | Sistema operacional convidado
------------- | ------------- | -----------
Discos rígidos virtuais compartilhados (somente VHDX) | 1 e 2  | Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012
SMB3 | 1 e 2 | Tudo que dá suporte a SMB3
Espaços de armazenamento diretos | 2 | Windows Server 2016
Fibre Channel Virtual | 1 e 2 | Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012
Formato VHDX | 1 e 2 | Qualquer convidado com suporte