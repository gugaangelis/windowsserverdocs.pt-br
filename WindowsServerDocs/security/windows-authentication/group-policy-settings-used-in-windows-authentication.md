---
title: "Configurações de política de grupo usadas na autenticação do Windows"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Configurações de política de grupo usadas na autenticação do Windows

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico de referência para o profissional de TI descreve o uso e o impacto das configurações de política de grupo no processo de autenticação.

Você pode gerenciar autenticação em sistemas operacionais Windows com a adição de usuário, computador e contas de serviço a grupos e pela aplicação de políticas de autenticação para esses grupos. Essas políticas são definidas como políticas de segurança local e como modelos administrativos, também conhecidos como configurações de política de grupo. Os dois conjuntos podem ser configurados e distribuídos na sua organização usando a política de grupo.

> [!NOTE]
> Recursos introduzidos no Windows Server 2012 R2, permitem que você configure políticas de autenticação para serviços de destino ou protegido de aplicativos, normalmente chamado silos de autenticação, usando contas. Para obter informações sobre como fazer isso no Active Directory, consulte [como configurar contas protegido](how-to-configure-protected-accounts.md).

Por exemplo, você pode aplicar as seguintes políticas a grupos, com base em sua função na organização:

-   Faça logon localmente ou em um domínio

-   Fazer logon em uma rede

-   Redefinir contas

-   Criar contas

A tabela a seguir lista os grupos de política relevantes para a autenticação e fornece links para documentação que podem ajudar você a configurar essas políticas.

|Grupo de política|Localização|Descrição|
|--------|------|--------|
|**Política de senha**|Políticas de conta do computador local diretiva Configuration\Windows Settings\Security|Políticas de senha afetam as características e comportamento de senhas. Políticas de senha são usadas para contas de domínio ou contas de usuário local. Elas determinam as configurações de senhas, como imposição e a vida útil.<br /><br />Para obter informações sobre configurações específicas, consulte [política de senha](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy).|
|**Política de bloqueio de conta**|Políticas de conta do computador local diretiva Configuration\Windows Settings\Security|Opções de política de bloqueio de conta desativam contas após um determinado número de tentativas de logon com falha. Usar essas opções pode ajudá-lo a detectar e bloquear tentativas para quebrar senhas.<br /><br />Para obter informações sobre opções de política de bloqueio de conta, consulte [política de bloqueio de conta](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy).|
|**Política de Kerberos**|Políticas de conta do computador local diretiva Configuration\Windows Settings\Security|Configurações relacionadas a Kerberos incluem as regras de tempo de vida e a imposição de tíquete. Política de Kerberos não se aplica aos bancos de dados de conta local como o protocolo de autenticação Kerberos não é usado para autenticar as contas locais. Portanto, as configurações de política de Kerberos podem ser configuradas somente por meio do objeto de política de grupo (GPO), onde ele afeta logons de domínio do domínio padrão.<br /><br />Para obter informações sobre opções de política de Kerberos para o controlador de domínio, consulte [política de Kerberos](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy).|
|**Política de auditoria**|Locais \ política de computador local diretiva Configuration\Windows Settings\Security|Política de auditoria permite controlar e entender o acesso a objetos, como arquivos e pastas e gerenciar contas de usuário e grupo e logons de usuário e logoffs. Políticas de auditoria podem especificar as categorias de eventos que você deseja auditar, defina o tamanho e o comportamento do log de segurança e determinar quais objetos você deseja monitorar o acesso e qual tipo de acesso que você deseja monitorar.<br /><br />|
|**Atribuição de direitos de usuário**|\ De diretiva de computador local \ Configurações de segurança atribuição de direitos do usuário de locais|Direitos de usuário normalmente são atribuídos com base dos grupos de segurança aos quais um usuário pertence, como os usuários, usuários avançados ou administradores. As configurações de política nesta categoria normalmente são usadas para conceder ou negar permissão para acessar um computador com base no método de acesso e segurança associações de grupo.|
|**Opções de segurança**|Computador local diretiva Configuration\Windows Settings\Security Settings\Local Policies\Security Options|Políticas relevantes para a autenticação incluem:<br /><br />-Dispositivos<br />-Controlador de domínio<br />-Membro do domínio<br />-Logon interativo<br />-Servidor de rede Microsoft<br />-Acesso à rede<br />-Segurança de rede<br />-Console de recuperação<br />-Desligamento<br /><br />|
|**Delegação de credenciais**|Computador configuração do computador\Modelos Templates\System\Credentials delegação|A delegação de credenciais é um mecanismo que permite credenciais locais a ser usado em outros sistemas, principalmente servidores membros e controladores de domínio em um domínio. Essas configurações se aplicam aos aplicativos usando as credenciais Security Support Provider (SSP credenciais). Conexão de área de trabalho remota é um exemplo.|
|**KDC**|Computador configuração do computador\Modelos Templates\System\KDC|Essas configurações de política afetam como a chave do Centro de distribuição (KDC), que é um serviço no controlador de domínio, manipula solicitações de autenticação Kerberos.|
|**Kerberos**|Computador configuração do computador\Modelos Templates\System\Kerberos|Essas configurações de política afetam como Kerberos está configurado para lidar com suporte para declarações, proteção Kerberos, autenticação composta, servidores proxy identificação e outras configurações.|
|**Logon**|Sistema de configuração do computador\Modelos do computador|Essas configurações de política controlam como o sistema apresenta a experiência de logon dos usuários.|
|**Logon de rede**|Configuração do computador\Modelos Templates\System\Net Logon no computador|Essas configurações de política controlam como o sistema manipula solicitações de logon de rede, incluindo como o localizador de controlador de domínio se comporta.<br /><br />Para saber mais sobre como o localizador de controlador de domínio se encaixa em processos de replicação, consulte [Noções básicas sobre a replicação entre Sites](https://technet.microsoft.com/library/cc771251.aspx).|
|**Biometria**|Computador Configuration\Administrative Templates\Windows Components\Biometrics|Em geral, essas configurações de política permitirem ou negar o uso de biometria como um método de autenticação.<br /><br />Para obter informações sobre a implementação do Windows de biometria, consulte Visão geral da estrutura biométrica Windows.|
|**Interface do usuário de credenciais**|Computador Configuration\Administrative Templates\Windows Windows\interface do usuário|Essas configurações de política controlam como as credenciais são gerenciadas no ponto de entrada.|
|**Sincronização de senhas**|Sincronização do computador Configuration\Administrative Templates\Windows Components\Password|Essas configurações de política determinam como o sistema gerencia a sincronização de senhas entre o Windows e sistemas operacionais baseados em UNIX.<br /><br />Para obter mais informações, consulte [sincronização de senhas](https://technet.microsoft.com/library/cc732609.aspx).|
|**Cartão inteligente**|Computador Configuration\Administrative Templates\Windows Components\Smart cartão|Essas configurações de política controlam como o sistema gerencia logons de cartão inteligente.<br /><br />|
|**Opções de Logon do Windows**|Opções de Logon do computador \ Modelos Administrativos\Componentes Windows|Essas configurações de política controlam quando e como oportunidades de logon estão disponíveis.|
|**Opções de Ctrl + Alt + Del**|Computador Configuration\Administrative Templates\Windows Components\Ctrl + Alt + Del opções|Essas configurações de política afetam a aparência de e o acesso aos recursos de logon da interface do usuário (área de trabalho protegida), como o Gerenciador de tarefas e o bloqueio de teclado do computador.|
|**Logon**|Computador Configuration\Administrative Templates\Windows Components\Logon|Essas configurações de política determinam se ou quais processos podem ser executados quando o usuário faz logon.|

## <a name="see-also"></a>Consulte também
[Visão geral técnica de autenticação do Windows](windows-authentication-technical-overview.md)


