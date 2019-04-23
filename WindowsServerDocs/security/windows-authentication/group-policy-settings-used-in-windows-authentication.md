---
title: Configurações da Política de Grupo usada na autenticação do Windows
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9e237f89-45b1-4a4e-9b72-11dc7d6a470b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 38df2033b57c0394b96f539f54efe6a3579500f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847927"
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Configurações da Política de Grupo usada na autenticação do Windows

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico de referência para profissionais de TI descreve o uso e o impacto das configurações de diretiva de grupo no processo de autenticação.

Você pode gerenciar autenticação nos sistemas operacionais do Windows, adicionando o usuário, computador e contas de serviço aos grupos e, em seguida, aplicando políticas de autenticação para esses grupos. Essas políticas são definidas como políticas de segurança local e como modelos administrativos, também conhecido como configurações de diretiva de grupo. Ambos os conjuntos podem ser configurados e distribuídos em toda a organização por meio de diretiva de grupo.

> [!NOTE]
> Recursos introduzidos no Windows Server 2012 R2, permitem que você configure as políticas de autenticação para serviços de destino ou contas protegidas de aplicativos, normalmente chamado de silos de autenticação com o uso. Para obter informações sobre como fazer isso no Active Directory, consulte [How to Configure Protected Accounts](how-to-configure-protected-accounts.md).

Por exemplo, você pode aplicar as políticas a seguir para grupos, com base em sua função na organização:

-   Faça logon localmente ou em um domínio

-   Faça logon em uma rede

-   Redefinir contas

-   Criar contas

A tabela a seguir lista os grupos de políticas relevantes para a autenticação e fornece links para a documentação que podem ajudar você a configurar essas políticas.

|Grupo de políticas|Location|Descrição|
|--------|------|--------|
|**Política de senha**|Políticas de conta do computador local política configuração \ Configurações de segurança|Políticas de senha afetam as características e o comportamento de senhas. Políticas de senha são usadas para contas de domínio ou contas de usuário local. Elas determinam as configurações para senhas, como o tempo de vida e a imposição.<br /><br />Para obter informações sobre configurações específicas, consulte [política de senha](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy).|
|**Política de bloqueio de conta**|Políticas de conta do computador local política configuração \ Configurações de segurança|Opções de política de bloqueio de conta desabilitam contas após um determinado número de tentativas de logon com falha. Usar essas opções pode ajudá-lo a detectar e bloquear tentativas de obtenção de senhas.<br /><br />Para obter informações sobre as opções de política de bloqueio de conta, consulte [política de bloqueio de conta](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy).|
|**Política do Kerberos**|Políticas de conta do computador local política configuração \ Configurações de segurança|Configurações relacionadas ao Kerberos incluem regras de tempo de vida e a imposição de tíquete. Política do Kerberos não é aplicável aos bancos de dados de conta local como o protocolo de autenticação Kerberos não é usado para autenticar contas locais. Portanto, as configurações de política do Kerberos podem ser configuradas por meio do grupo de política de GPO (objeto), em que ele afeta logons de domínio do domínio padrão.<br /><br />Para obter informações sobre as opções de política do Kerberos para o controlador de domínio, consulte [política do Kerberos](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy).|
|**A política de auditoria**|Computador local política configuração \ Configurações de segurança \ Diretivas locais|Política de auditoria permite que você controle e entender o acesso a objetos, como arquivos e pastas e para gerenciar contas de usuário e grupo e usuário logons e logoffs. Políticas de auditoria podem especificar as categorias de eventos que você deseja auditar, defina o tamanho e o comportamento do log de segurança e determinar de quais objetos você deseja monitorar o acesso e que tipo de acesso que você deseja monitorar.<br /><br />|
|**Atribuição de direitos de usuário**|Diretiva computador local Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment|Direitos de usuário normalmente são atribuídos com base nos grupos de segurança aos quais um usuário pertence, como administradores, usuários avançados ou usuários. As configurações de política nessa categoria normalmente são usadas para conceder ou negar permissão para acessar um computador com base no método de acesso e segurança de associações de grupo.|
|**Opções de segurança**|Diretiva de computador local Configuration\Windows Settings\Security Settings\Local Policies\Security Options|As políticas relevantes para a autenticação incluem:<br /><br />-Devices<br />-Controlador de domínio<br />-Membro do domínio<br />-Logon interativo<br />-Servidor de rede Microsoft<br />-Acesso à rede<br />-Segurança de rede<br />-Console de recuperação<br />-Shutdown<br /><br />|
|**Delegação de credenciais**|Delegação de políticas de configuração do computador|A delegação de credenciais é um mecanismo que permite que as credenciais locais seja usado em outros sistemas, especialmente servidores membros e controladores de domínio dentro de um domínio. Essas configurações se aplicam a aplicativos usando o Credential Security Support Provider (SSP Cred). Conexão de área de trabalho remota é um exemplo.|
|**KDC**|Templates\System\KDC de configuração do computador|Essas configurações de diretiva afetam como o KDC Key Distribution Center (), que é um serviço no controlador de domínio, trata as solicitações de autenticação Kerberos.|
|**Kerberos**|Templates\System\Kerberos de configuração do computador|Essas configurações de diretiva afetam como o Kerberos é configurado para lidar com suporte para declarações, a proteção Kerberos, autenticação composta, servidores de proxy de identificação e outras configurações.|
|**Logon**|Configuração do Computador\Modelos Administrativos\Sistema\Logon|Essas configurações controlam como o sistema apresenta a experiência de logon dos usuários.|
|**Logon de rede**|Configuração do computador\Modelos Templates\System\Net Logon no computador|Essas configurações controlam como o sistema lida com solicitações de logon de rede, incluindo como o localizador de controlador de domínio se comporta.<br /><br />Para obter mais informações sobre como o localizador de controlador de domínio se encaixa processos de replicação, consulte [Noções básicas sobre a replicação entre Sites](https://technet.microsoft.com/library/cc771251.aspx).|
|**Biometria**|Computador Configuration\Administrative Templates\Windows Components\Biometrics|Em geral, essas configurações de política permitem ou negar o uso de biometria como um método de autenticação.<br /><br />Para obter informações sobre a implementação de biometria do Windows, consulte Visão geral Windows Biometric Framework.|
|**Interface de usuário da credencial**|Computador Configuration\Administrative Templates\Windows Windows\interface do usuário|Essas configurações controlam como as credenciais são gerenciadas no ponto de entrada.|
|**Sincronização de senha**|Sincronização do computador Configuration\Administrative Templates\Windows Components\Password|Essas configurações de política determinam como o sistema gerencia a sincronização de senhas entre sistemas operacionais baseados em UNIX e o Windows.<br /><br />Para obter mais informações, consulte [sincronização de senha](https://technet.microsoft.com/library/cc732609.aspx).|
|**Cartão inteligente**|Computador Configuration\Administrative Templates\Windows Components\Smart cartão|Essas configurações controlam como o sistema gerencia os logons de cartão inteligente.<br /><br />|
|**Opções de Logon do Windows**|Opções de Logon do computador Configuration\Administrative Templates\Windows modelos|Essas configurações controlam quando e como oportunidades de logon estão disponíveis.|
|**Opções de Ctrl + Alt + Del**|Opções do computador Configuration\Administrative Templates\Windows Components\Ctrl + Alt + Del|Essas configurações de diretiva afetam a aparência do e a acessibilidade aos recursos no logon da interface do usuário (área de trabalho protegida), como o Gerenciador de tarefas e o bloqueio do teclado do computador.|
|**Logon**|Computador Configuration\Administrative Templates\Windows Components\Logon|Essas configurações de política determinam se ou quais processos podem ser executado quando o usuário fizer logon.|

## <a name="see-also"></a>Consulte também
[Visão geral técnica de autenticação do Windows](windows-authentication-technical-overview.md)


