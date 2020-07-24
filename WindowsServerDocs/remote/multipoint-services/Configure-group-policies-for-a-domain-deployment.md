---
title: Configurar políticas de grupo para uma implantação de domínio
description: Saiba como configurar políticas de grupo nos serviços do MultiPoint
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 13e5fa90-d330-4155-a6b8-78eb650cbbfa
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 9c01bdd45b5aa88a8ce4f8a5876a0f3cbc3c0cc3
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966308"
---
# <a name="configure-group-policies-for-a-domain-deployment"></a>Configurar políticas de grupo para uma implantação de domínio
Para garantir que a implantação de domínio dos serviços do MultiPoint funcione corretamente, aplique as seguintes configurações de política de grupo à conta de usuário WMSshell em um sistema MultiPoint Services.  
  
> [!IMPORTANT]  
> Algumas configurações de diretiva de grupo podem impedir que as definições de configuração necessárias sejam aplicadas aos serviços do MultiPoint. Certifique-se de entender e definir as configurações da política de grupo para que elas funcionem corretamente nos serviços do MultiPoint. Por exemplo, uma configuração Política de Grupo que impede que o logon automático possa apresentar problemas com o comportamento de login dos serviços do MultiPoint.  
  
## <a name="update-group-policies-for-the-wmsshell-user-account"></a>Atualizar políticas de grupo para a conta de usuário do WMSshell 
A conta de usuário do WMSshell é uma conta do sistema que o MultiPoint Services usa para entrar no console do, em que as estações reais são criadas. Essa conta não deve ser gerenciada pelo MultiPoint Manager.
  
> [!NOTE]  
> Para saber como atualizar as políticas de grupo, consulte [Editor de política de grupo local](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265982(v=ws.11)).  
  
**Política:** Configuração do usuário > modelos administrativos > painel de controle > **personalização**  
  
Atribua os seguintes valores:  
  
|Configuração|Valores|  
|-----------|----------|  
|Enable screen saver|Desabilitado|  
|Tempo limite de Proteção de Tela|Desabilitado<p>Segundos: xxx|  
|Proteger com senha a proteção de tela|Desabilitado|  
  
**Política:** Configuração do computador >configurações do Windows >configurações de segurança >políticas locais >atribuição de direitos de usuário > **Permitir logon local**  
  
|Configuração|Valores|  
|-----------|----------|  
|Permitir logon localmente|Verifique se a lista de contas inclui a conta WMSshell.<p>**Observação:** Por padrão, a conta WMSshell é um membro do grupo usuários. Se o grupo usuários estiver na lista e WMSshell for um membro do grupo usuários, você não precisará adicionar a conta WMSshell à lista.|  
  
> [!IMPORTANT]  
> Ao definir qualquer política de grupo, verifique se as políticas não interferem nas atualizações automáticas e no relatório de erros do Windows no MultiPoint Server. Elas são definidas pelas configurações **instalar atualizações automaticamente** e **automáticas do relatório de erros do Windows** que foram selecionadas durante a instalação do Windows MultiPoint Server, configuradas no MultiPoint Manager usando **Editar configurações do servidor**ou configuradas em atualizações agendadas para proteção de disco.  
  
## <a name="update-the-registry"></a>Atualizar o registro  
Para uma implantação de domínio dos serviços do MultiPoint, você deve atualizar as seguintes subchaves do registro.  
  
> [!IMPORTANT]  
> A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.  
  
#### <a name="to-update-registry-subkeys-for-a-domain-deployment-of-multipoint-services"></a>Para atualizar as subchaves do registro para uma implantação de domínio dos serviços do MultiPoint  
  
1.  Abra o editor do registro. (Em um prompt de comando, digite **regedit.exe**e pressione Enter.)  
  
2.  No painel esquerdo, localize e selecione a seguinte subchave do registro:  
  
    HKEY_USERS \<SIDofWMSshell> \Software\Policies\Microsoft\Windows\Control Panel\Desktop  
  
    onde ' <SIDofWMSshell> ' é o identificador de segurança (SID) da conta WMSshell. Para descobrir como identificar o SID, consulte [como associar um nome de usuário a um SID (identificador de segurança)](https://support.microsoft.com/kb/154599).  
  
3.  Na lista à direita, atualize as seguintes subchaves.  
  
    |Subchave|Nome do valor|Dados do valor|  
    |----------|--------------|--------------|  
    |ScreenSaveActive|REG_SZ|0 (zero)|  
    |ScreenSaveTimeout|REG_SZ|120|  
    |ScreenSaverIsSecure|REG_SZ|0 (zero)|  
  
    Para atualizar uma subchave do registro:  
  
    1.  Com a chave do registro selecionada no painel esquerdo, clique com o botão direito do mouse na subchave no painel direito e clique em **Modificar**.  
  
    2.  Na caixa de diálogo Editar Cadeia de caracteres, digite um novo valor em **dados de valor**e clique em **OK**.  
  
4.  Depois de concluir a atualização das subchaves do registro, reinicie o computador para ativar as alterações. 
