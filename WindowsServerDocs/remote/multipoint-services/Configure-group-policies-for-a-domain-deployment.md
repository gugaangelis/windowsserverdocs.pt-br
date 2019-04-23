---
title: Configurar políticas de grupo para uma implantação de domínio
description: Saiba como configurar políticas de grupo no MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13e5fa90-d330-4155-a6b8-78eb650cbbfa
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: f661fbdc40fd7dd2562d51756bc7642c8e9a4a82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888037"
---
# <a name="configure-group-policies-for-a-domain-deployment"></a>Configurar políticas de grupo para uma implantação de domínio
Para garantir que sua implantação do domínio do MultiPoint Services funcione corretamente, aplique as seguintes configurações de política de grupo para a conta de usuário WMSshell em um sistema MultiPoint Services.  
  
> [!IMPORTANT]  
> Algumas configurações de diretiva de grupo podem impedir que as definições de configuração necessárias sejam aplicados aos MultiPoint Services. Certifique-se de que você compreenda e define as configurações de diretiva de grupo para que eles funcionem corretamente no MultiPoint Services. Por exemplo, uma configuração de diretiva de grupo que impede o logon automático pode apresentar problemas com o comportamento de logon do MultiPoint Services.  
  
## <a name="update-group-policies-for-the-wmsshell-user-account"></a>Atualizar as políticas de grupo para a conta de usuário WMSshell 
A conta de usuário WMSshell é uma conta de sistema quais serviços MultiPoint usa para entrar no console onde as estações actuall são criadas. Essa conta não é mento ser gerenciado pelo Gerenciador do MultiPoint.
  
> [!NOTE]  
> Para saber como atualizar as diretivas de grupo, consulte [Editor de diretiva de Grupo Local](https://technet.microsoft.com/library/dn265982.aspx).  
  
**POLÍTICA:** Configuração do usuário > modelos administrativos > Painel de controle > **personalização**  
  
Atribua valores a seguir:  
  
|Configuração|Valores|  
|-----------|----------|  
|Habilitar a proteção de tela|Desabilitada|  
|Tempo limite de Proteção de Tela|Desabilitada<br /><br />Segundos: xxx|  
|Proteger com senha a proteção de tela|Desabilitada|  
  
**POLÍTICA:** Configuração do computador > configurações do Windows > configurações de segurança > políticas locais > atribuição de direitos de usuário > **permitir logon localmente**  
  
|Configuração|Valores|  
|-----------|----------|  
|Permitir logon localmente|Certifique-se de que a lista de contas inclui a conta WMSshell.<br /><br />**Observação:** Por padrão, a conta de WMSshell é um membro do grupo de usuários. Se o grupo de usuários está na lista e WMSshell é um membro do grupo de usuários, você precisa adicionar a conta WMSshell à lista.|  
  
> [!IMPORTANT]  
> Quando você define políticas de grupo, certifique-se de que as políticas não interfiram com atualizações automáticas e erro do Windows erro reporting no servidor do MultiPoint. Elas são definidas **instalar atualizações automaticamente** e **relatório de erros do Windows automáticas** configurações que foram selecionadas durante a instalação do Windows MultiPoint Server, configurada no MultiPoint Usando o Gerenciador **editar configurações de servidor**, ou configurado em atualizações agendadas para a proteção de disco.  
  
## <a name="update-the-registry"></a>Atualizar o registro  
Para uma implantação do domínio do MultiPoint Services, você deve atualizar as seguintes subchaves do registro.  
  
> [!IMPORTANT]  
> A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.  
  
#### <a name="to-update-registry-subkeys-for-a-domain-deployment-of-multipoint-services"></a>Para atualizar as subchaves do registro para uma implantação do domínio do MultiPoint Services  
  
1.  Abra o editor do registro. (Em um prompt de comando, digite **regedit.exe**, e pressione ENTER.)  
  
2.  No painel esquerdo, localize e, em seguida, selecione a seguinte subchave do registro:  
  
    HKEY_USERS\<SIDofWMSshell > \Software\Policies\Microsoft\Windows\Control Panel\Desktop  
  
    onde '<SIDofWMSshell>' é o identificador de segurança (SID) para a conta WMSshell. Para saber como identificar o SID, consulte [como associar um nome de usuário com um identificador de segurança (SID)](https://support.microsoft.com/kb/154599).  
  
3.  Na lista à direita, atualize as seguintes subchaves.  
  
    |Subchave|Nome do valor|os dados de Valor|  
    |----------|--------------|--------------|  
    |ScreenSaveActive|REG_SZ|0 (zero)|  
    |ScreenSaveTimeout|REG_SZ|120|  
    |ScreenSaverIsSecure|REG_SZ|0 (zero)|  
  
    Para atualizar uma subchave do registro:  
  
    1.  Com a chave do registro selecionada no painel esquerdo, clique com botão direito na subchave no painel direito e, em seguida, clique em **modificar**.  
  
    2.  Na caixa de diálogo Editar cadeia de caracteres, digite um novo valor na **dados do valor**e, em seguida, clique em **Okey**.  
  
4.  Depois de concluir a atualização de subchaves do registro, reinicie o computador para ativar as alterações. 