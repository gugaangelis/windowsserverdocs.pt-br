---
title: Recursos removidos ou preteridos no Windows Server 2016
description: Uma lista de recursos e funcionalidades no Windows Server 2016 que foram removidas do produto na versão atual ou estão previstas para possível remoção em versões futuras (preteridos). Essa lista é direcionada para profissionais de TI que estão atualizando sistemas operacionais em um ambiente comercial.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.date: 05/21/2019
ms.assetid: 5d10c5f9-ebac-49a0-b808-c0b1702e0437
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 83855cf7e4fa86a932298dd15735dc5bf7277dfb
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976602"
---
# <a name="features-removed-or-deprecated-in--windows-server-2016"></a>Recursos removidos ou preteridos no Windows Server 2016

>Aplica-se a: Windows Server 2016

Veja a seguir uma lista de recursos e funcionalidades do Windows Server 2016 que foram removidos do produto na versão atual ou estão previstos para possível remoção em versões futuras (preteridos). Essa lista é direcionada para profissionais de TI que estão atualizando sistemas operacionais em um ambiente comercial. Esta lista está sujeita à alteração nas versões subsequentes e pode não incluir todos os recursos ou funcionalidades preteridas. Para obter mais detalhes sobre determinado recurso ou funcionalidade e sua substituição, consulte a documentação desse recurso.

Para obter informações sobre o que foi removido ou preterido nas versões mais recentes, consulte [recursos removidos ou planejados para substituição iniciando o Windows Server 2019](../get-started-19/removed-features-19.md).

## <a name="features-removed-from-windows-server-2016"></a>Recursos removidos do Windows Server 2016

Os recursos e funcionalidades a seguir foram retirados desta versão do Windows Server 2016. Aplicativos, códigos ou o uso que dependem desses recursos não funcionarão nesta versão a menos que você implante um método alternativo.  

> [!NOTE]  
> Se você estiver migrando para o Windows Server 2016 de uma versão de servidor anterior ao Windows Server 2012 R2 ou Windows Server 2012, leia também os tópicos [Recursos removidos ou preteridos no Windows Server 2012 R2](https://technet.microsoft.com/library/dn303411.aspx) e [Recursos removidos ou preteridos no Windows Server 2012](https://technet.microsoft.com/library/hh831568.aspx).  


### <a name="file-server"></a>Servidor de arquivos  
O snap-in de Gerenciamento de Compartilhamento e Armazenamento para o Microsoft Management Console foi removido. Em vez disso, siga qualquer um destes procedimentos:  

-   Se o computador que você deseja gerenciar está executando um sistema operacional mais antigo que o Windows Server 2016, conecte-o com a Área de Trabalho Remota e use a versão local do snap-in de Gerenciamento de Compartilhamento e Armazenamento.  

-   Em um computador que executa o Windows 8.1 ou anterior, use o snap-in de Gerenciamento de Compartilhamento e Armazenamento do RSAT para ver o computador que você deseja gerenciar.  

-   Use o Hyper-V em um computador cliente para executar uma máquina virtual que executa o Windows 7, Windows 8 ou Windows 8.1 com o snap-in de Gerenciamento de Compartilhamento e Armazenamento no RSAT.  

### <a name="journaldll"></a>Journal.dll  
Journal.dll foi removido do Windows Server 2016. Não há nenhum substituto.  

### <a name="security-configuration-wizard"></a>Assistente de Configuração de Segurança  
O Assistente de Configuração de Segurança foi removido. Em vez disso, os recursos são protegidos por padrão. Se você precisar controlar configurações específicas de segurança, pode usar a Política de Grupo ou [Microsoft Security Compliance Manager](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx).  

### <a name="sqm"></a>SQM  
Os componentes de aceitação que gerenciam a participação no Programa de Aperfeiçoamento da Experiência do Usuário foram removidos. 

### <a name="windows-update"></a>Windows Update
O comando **wuauclt.exe /detectnow** foi removido e não é mais compatível. Para disparar uma verificação de atualizações, siga um destes procedimentos:

- Execute estes comandos do PowerShell:
    ````powershell
    $AutoUpdates = New-Object -ComObject "Microsoft.Update.AutoUpdate"`
    $AutoUpdates.DetectNow()` 
    ````

- Como alternativa, use este VBScript:
    ````vb
    Set automaticUpdates = CreateObject("Microsoft.Update.AutoUpdate")
    automaticUpdates.DetectNow()
    ````

## <a name="features-deprecated-starting-with-windows-server-2016"></a>Recursos preteridos a partir do Windows Server 2016 
Os seguintes recursos e funcionalidades foram preteridos a partir desta versão. Eventualmente, eles serão completamente removidos do produto, mas ainda estarão disponíveis nesta versão, algumas vezes, com determinadas funcionalidades removidas. Comece a planejar agora a implantação de métodos alternativos para qualquer aplicativo, código, ou utilização que dependa desses recursos.  

### <a name="configuration-tools"></a>Ferramentas de configuração  

-   **Scregedit.exe** foi preterido. Se você tiver scripts que dependem de Scregedit.exe, ajuste-os para usar os métodos Reg.exe ou Windows PowerShell.  

-   **Sconfig.exe** foi preterido. Use o Windows PowerShell.  

### <a name="netcfg-custom-apis"></a>APIs personalizadas de NetCfg  
A instalação de PrintProvider, NetClient e ISDN usando APIs personalizadas de NetCfg foi preterida.  

### <a name="remote-management"></a>Gerenciamento remoto  
WinRM.vbs foi preterido. Em vez disso, use a funcionalidade no provedor de WinRM do Windows PowerShell.  

### <a name="smb"></a>SMB  
SMB2+ sobre NetBT foi preterido. Em vez disso, implemente SMB sobre TCP ou RDMA. 
