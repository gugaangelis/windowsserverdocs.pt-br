---
title: Configurações da Política de Grupo usada na autenticação do Windows
description: Segurança do Windows Server
ms.prod: windows-server
ms.technology: security-windows-auth
ms.topic: article
ms.assetid: 9e237f89-45b1-4a4e-9b72-11dc7d6a470b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 9cbf10e9ebce5bbe0865f28001d0c505b42c9742
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517991"
---
# <a name="group-policy-settings-used-in-windows-authentication"></a>Configurações da Política de Grupo usada na autenticação do Windows

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico de referência para o profissional de ti descreve o uso e o impacto das configurações de Política de Grupo no processo de autenticação.

Você pode gerenciar a autenticação em sistemas operacionais Windows adicionando contas de usuário, computador e serviço a grupos e, em seguida, aplicando políticas de autenticação a esses grupos. Essas políticas são definidas como políticas de segurança local e como modelos administrativos, também conhecidos como configurações de Política de Grupo. Os dois conjuntos podem ser configurados e distribuídos em toda a sua organização usando Política de Grupo.

> [!NOTE]
> Os recursos introduzidos no Windows Server 2012 R2 permitem configurar políticas de autenticação para serviços de destino ou aplicativos, normalmente chamados de silos de autenticação, usando contas protegidas. Para obter informações sobre como fazer isso no Active Directory, consulte [como configurar contas protegidas](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/how-to-configure-protected-accounts).

Por exemplo, você pode aplicar as seguintes políticas a grupos, com base em sua função na organização:

-   Fazer logon localmente ou em um domínio

-   Fazer logon em uma rede

-   Redefinir contas

-   Criar contas

A tabela a seguir lista grupos de políticas relevantes para autenticação e fornece links para documentação que podem ajudá-lo a configurar essas políticas.

|Grupo de políticas|Location|Descrição|
|--------|------|--------|
|**Política de senha**|Computador local Policy\Computer \ \ políticas de Settings\Account|As políticas de senha afetam as características e o comportamento das senhas. As políticas de senha são usadas para contas de domínio ou contas de usuário local. Eles determinam as configurações de senhas, como imposição e tempo de vida.<p>Para obter informações sobre configurações específicas, consulte [política de senha](https://technet.microsoft.com/itpro/windows/keep-secure/password-policy).|
|**Política de bloqueio de conta**|Computador local Policy\Computer \ \ políticas de Settings\Account|Opções de política de bloqueio de conta desabilite contas após um número definido de tentativas de logon com falha. O uso dessas opções pode ajudá-lo a detectar e bloquear tentativas de quebra de senhas.<p>Para obter informações sobre opções de política de bloqueio de conta, consulte [política de bloqueio de conta](https://technet.microsoft.com/itpro/windows/keep-secure/account-lockout-policy).|
|**Política do Kerberos**|Computador local Policy\Computer \ \ políticas de Settings\Account|As configurações relacionadas a Kerberos incluem o tempo de vida de tíquete e as regras de imposição. A política Kerberos não se aplica a bancos de dados de contas locais porque o protocolo de autenticação Kerberos não é usado para autenticar contas locais. Portanto, as configurações de política do Kerberos podem ser configuradas apenas por meio do GPO (objeto de Política de Grupo de domínio) padrão, onde afeta os logons de domínio.<p>Para obter informações sobre as opções de política do Kerberos para o controlador de domínio, consulte [política do Kerberos](https://technet.microsoft.com/itpro/windows/keep-secure/kerberos-policy).|
|**Política de auditoria**|Política do computador local Policy\Computer \ Configurações do Policies\Audit|A política de auditoria permite controlar e entender o acesso a objetos, como arquivos e pastas, e gerenciar contas de usuário e de grupo e logons e logoffs de usuário. As políticas de auditoria podem especificar as categorias de eventos que você deseja auditar, definir o tamanho e o comportamento do log de segurança e determinar quais objetos você deseja monitorar o acesso e qual tipo de acesso você deseja monitorar.<p>|
|**Atribuição de Direitos do Usuário**|Computador local Policy\Computer \ \ Configurações de atribuição de direitos \ Computer|Os direitos de usuário são normalmente atribuídos com base nos grupos de segurança aos quais um usuário pertence, como administradores, usuários avançados ou usuários. As configurações de política nessa categoria normalmente são usadas para conceder ou negar permissão para acessar um computador com base no método de associações de grupo de acesso e segurança.|
|**Opções de segurança**|Opções do computador local Policy\Computer \ \ Configurações de \ \ \ Configurações|As políticas relevantes para a autenticação incluem:<p>-Dispositivos<br />-Controlador de domínio<br />-Membro do domínio<br />-Logon interativo<br />-Servidor de rede da Microsoft<br />-Acesso à rede<br />-Segurança de rede<br />-Console de recuperação<br />-Desligar<p>|
|**Delegação de credenciais**|Delegação do Computador\modelos Templates\System\Credentials|A delegação de credenciais é um mecanismo que permite que as credenciais locais sejam usadas em outros sistemas, principalmente em servidores membros e controladores de domínio em um domínio. Essas configurações se aplicam a aplicativos usando o provedor de suporte de segurança de credencial (cred SSP). Conexão de Área de Trabalho Remota é um exemplo.|
|**KDC**|Configuração do Computador\modelos Templates\System\KDC|Essas configurações de política afetam como o centro de distribuição de chaves (KDC), que é um serviço no controlador de domínio, lida com as solicitações de autenticação Kerberos.|
|**Kerberos**|Configuração do Computador\modelos Templates\System\Kerberos|Essas configurações de política afetam o modo como o Kerberos é configurado para lidar com suporte a declarações, proteção Kerberos, autenticação composta, identificação de servidores proxy e outras configurações.|
|**Logon**|Configuração do Computador\Modelos Administrativos\Sistema\Logon|Essas configurações de política controlam como o sistema apresenta a experiência de logon para os usuários.|
|**Logon de Rede**|Computador \ Configuração de logon do Computador\modelos Templates\System\Net|Essas configurações de política controlam como o sistema trata as solicitações de logon de rede, incluindo como o localizador do controlador de domínio se comporta.<p>Para obter mais informações sobre como o localizador do controlador de domínio se encaixa em processos de replicação, consulte [noções básicas sobre replicação entre sites](https://technet.microsoft.com/library/cc771251.aspx).|
|**Biometria**|Computador \ Configuração do Computador\modelos Administrativos\Componentes do Components\Biometrics|Essas configurações de política geralmente permitem ou negam o uso da biometria como um método de autenticação.<p>Para obter informações sobre a implementação de biometria do Windows, consulte Windows Biometric Framework visão geral.|
|**Interface do usuário de credencial**|Computer configuração do Computador\modelos Administrativos\Componentes do Components\Credential interface do usuário|Essas configurações de política controlam como as credenciais são gerenciadas no ponto de entrada.|
|**Sincronização de senha**|Configuração do Computador\modelos Administrativos\Componentes do Components\Password|Essas configurações de política determinam como o sistema gerencia a sincronização de senhas entre sistemas operacionais baseados em Windows e UNIX.<p>Para obter mais informações, consulte [sincronização de senha](https://technet.microsoft.com/library/cc732609.aspx).|
|**Cartão inteligente**|Configuração do Computador\modelos Administrativos\Componentes do Components\Smart Card|Essas configurações de política controlam como o sistema gerencia logons de cartão inteligente.<p>|
|**Opções de logon do Windows**|Configuração do Computador\modelos Administrativos\Componentes do Windows\windows opções de logon|Essas configurações de política controlam quando e como as oportunidades de logon estão disponíveis.|
|**Opções Ctrl + Alt + Del**|Configuração do Computador\modelos Administrativos\Componentes do Components\Ctrl + alt + del|Essas configurações de diretiva afetam a aparência e a acessibilidade aos recursos na interface do usuário de logon (área de trabalho segura), como o Gerenciador de tarefas e o bloqueio de teclado do computador.|
|**Logon**|Computador \ Configuração do Computador\modelos Administrativos\Componentes do Components\Logon|Essas configurações de política determinam se ou quais processos podem ser executados quando o usuário faz logon.|

## <a name="additional-references"></a>Referências adicionais
[Visão geral técnica de autenticação do Windows](windows-authentication-technical-overview.md)


