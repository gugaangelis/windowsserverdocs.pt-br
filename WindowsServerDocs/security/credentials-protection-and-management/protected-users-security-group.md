---
title: Grupo de segurança de usuários protegidos
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f296
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: bd6b53c0febdfb2d344136097a9654c981405568
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862357"
---
# <a name="protected-users-security-group"></a>Grupo de segurança de usuários protegidos

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico destinado a profissionais de TI descreve o grupo de segurança do Active Directory chamado Usuários Protegidos e explica como ele funciona. Este grupo foi introduzido em controladores de domínio do Windows Server 2012 R2.

## <a name="BKMK_ProtectedUsers"></a>Visão geral

Esse grupo de segurança foi criado como parte de uma estratégia para gerenciar a exposição de credencial dentro da empresa. Os membros deste grupo possuem proteções não configuráveis automaticamente aplicadas às suas contas. A associação ao grupo Usuários protegidos visa restringir e proteger proativamente por padrão. O único método de modificar tais proteções de uma conta é removê-la do grupo de segurança.

> [!WARNING]
> Contas de serviços e computadores nunca deverão ser membros do grupo usuários protegidos. Este grupo seria fornece proteção incompleta mesmo assim porque a senha ou certificado sempre está disponível no host. A autenticação falhará com o erro \"o nome de usuário ou senha está incorreta\" para qualquer serviço ou um computador que é adicionado ao grupo usuários protegidos.

Este grupo global e relacionado ao domínio aciona proteções não configuráveis em dispositivos e computadores host que executam o Windows Server 2012 R2 e Windows 8.1 ou posterior para usuários em domínios com um controlador de domínio executando o Windows Server 2012 R2. Isso reduz consideravelmente o volume de memória padrão de credenciais quando os usuários logon em computadores com essas proteções.

Para obter mais informações, consulte [como os usuários protegidos agrupar works](#BKMK_HowItWorks) neste tópico.



## <a name="BKMK_Requirements"></a>Requisitos de grupo de usuários protegidos
Os requisitos para fornecer proteções do dispositivo para membros do grupo usuários protegidos são:

- O grupo de segurança global de Usuários protegidos é replicado para todos os controladores de domínio na conta de domínio.

- Windows 8.1 e Windows Server 2012 R2 adicionou suporte por padrão. [Microsoft Security Advisory 2871997](https://technet.microsoft.com/library/security/2871997) adiciona o suporte ao Windows 7, Windows Server 2008 R2 e Windows Server 2012.

Os requisitos para fornecer a proteção ao controlador de domínio para membros do grupo de Usuários protegidos são:

- Os usuários devem estar em domínios que são do Windows Server 2012 R2 ou superior nível funcional do domínio.

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>Adicionando grupo de segurança global de usuários protegidos para domínios de nível inferior

Controladores de domínio que executam um sistema operacional anterior ao Windows Server 2012 R2 podem dar suporte a adição de membros para o novo grupo de segurança usuários protegidos. Isso permite que os usuários se beneficiem de proteções de dispositivo antes de atualizar o domínio. 

> [!Note]
> Os controladores de domínio não suportará as proteções de domínio. 

Grupo de usuários protegidos pode ser criado por [transferir a função de emulador PDC (controlador) de domínio primário](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) para um controlador de domínio que executa o Windows Server 2012 R2. Depois de o objeto do grupo ser replicado para outros controladores de domínio, a função do emulador de PDC pode se hospedada em um controlador de domínio que executa uma versão anterior do Windows Server.

### <a name="BKMK_ADgroup"></a>Propriedades do grupo do AD de usuários protegidas

A tabela a seguir especifica as propriedades do grupo Usuários protegidos.

|Atributo|Valor|
|-------|-----|
|SID/RID conhecido|S-1-5-21-<domain>-525|
|Tipo|Domínio global|
|Contêiner padrão|CN=Users, DC=<domain>, DC=|
|Membros padrão|Nenhuma|
|Membro padrão de|Nenhuma|
|Protegido por ADMINSDHOLDER?|Não|
|É seguro movê-lo para fora do contêiner padrão?|Sim|
|É seguro delegar o gerenciamento deste grupo a administradores que não são de serviço?|Não|
|Direitos de usuário padrão|Nenhum direito de usuário padrão.|

## <a name="BKMK_HowItWorks"></a>Como o grupo usuários protegidos funciona
Essa seção explica como o grupo Usuários protegidos funciona quando:

-   Conectado um dispositivo Windows

-   Domínio da conta de usuário está em um Windows Server 2012 R2 ou o mais alto nível funcional do domínio

### <a name="device-protections-for-signed-in-protected-users"></a>Proteções de dispositivo para entrar usuários protegidos
Quando o usuário conectado é membro do grupo usuários protegidos são aplicadas as proteções do seguintes:

-   Credencial será de delegação (CredSSP) armazena em cache as credenciais do usuário texto sem formatação, mesmo quando o **permitir delegação de credenciais padrão** configuração de diretiva de grupo está habilitada.

-   Começando com o Windows 8.1 e Windows Server 2012 R2, Windows Digest não armazenará em cache as credenciais do usuário texto sem formatação, mesmo quando o Windows Digest está habilitado.

> [!Note]
> Depois de instalar [Microsoft Security Advisory 2871997](https://technet.microsoft.com/library/security/2871997) Windows Digest continuará em cache as credenciais, até que a chave do registro está configurada. Consulte [Microsoft Security Advisory: Atualização para melhorar a proteção e gerenciamento de credenciais: 13 de maio de 2014](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a) para obter instruções.

-   NTLM não armazenará em cache as credenciais de texto sem formatação do usuário ou função unidirecional do NT (NTOWF).

-   Kerberos não poderá mais criar chaves DES ou RC4. Também ele não armazenará em cache as credenciais de texto sem formatação ou chaves de longo prazo do usuário depois que o TGT inicial é adquirido.

-   Um verificador armazenado em cache não é criado ao entrar ou desbloquear, portanto, entrar offline não é mais suportada.

Depois que a conta de usuário é adicionada ao grupo usuários protegidos, a proteção será iniciado quando o usuário faz logon no dispositivo.

### <a name="domain-controller-protections-for-protected-users"></a>Proteções de controlador de domínio para usuários protegidos
As contas que são membros do grupo usuários protegidos que se autenticarem a um domínio do Windows Server 2012 R2 não for possível:

-   Autenticar-se com autenticação NTLM.

-   Usar os tipos de criptografia DES ou RC4 na pré-autenticação do Kerberos.

-   Ser delegadas com delegação restrita ou irrestrita.

-   Renovar os TGTs do Kerberos além do prazo inicial de quatro horas.

As definições não configuráveis para a expiração de TGTs são estabelecidas para cada conta do grupo de Usuários protegidos. Normalmente, o controlador de domínio define o tempo de vida e renovação do TGT com base nas políticas do domínio **Tempo de vida máximo para o tíquete do usuário** e **Tempo de vida máximo para renovação de tíquete do usuário**. Para o grupo de Usuários protegidos, foram definidos 600 minutos para essas políticas de domínio.

Para mais informações, consulte [Como configurar contas protegidas](how-to-configure-protected-accounts.md).

## <a name="troubleshooting"></a>Solução de problemas
Dois logs administrativos operacionais estão disponíveis para ajudar a solucionar problemas de eventos relacionados aos Usuários protegidos. Esses novos logs estão no Visualizador de eventos e são desabilitad por padrão, localizados em **Applications and Services Logs\Microsoft\Windows\Microsoft\Authentication**.

|ID e log de evento|Descrição|
|----------|--------|
|104<br /><br />**ProtectedUser-Client**|Motivo: O pacote de segurança no cliente não contém as credenciais.<br /><br />O erro é registrado em log no computador cliente quando a conta é um membro do grupo de segurança Usuários protegidos. Este evento indica que o pacote de segurança não armazena em cache as credenciais necessárias para autenticar o servidor.<br /><br />Exibe os nomes do pacote, usuário, domínio e servidor.|
|304<br /><br />**ProtectedUser-Client**|Motivo: O pacote de segurança não armazena credenciais do usuário protegido.<br /><br />Um evento informativo é registrado no cliente para indicar que o pacote de segurança não armazena em cache credenciais de logon do usuário. Espera-se que o Digest (WDigest), Delegação de credenciais (CredSSP) e NTLM falhem ao entrar com credenciais para Usuários protegidos. Os aplicativos ainda funcionarão se solicitarem as credenciais.<br /><br />Exibe os nomes do pacote, usuário e domínio.|
|100<br /><br />**ProtectedUserFailures-DomainController**|Motivo: Uma falha de logon de NTLM que ocorre em uma conta presente no grupo de segurança Usuários protegidos.<br /><br />Um erro é registrado em log no controlador de domínio para indicar que a autenticação NTLM falhou porque a conta era membro do grupo de segurança Usuários protegidos.<br /><br />Exibe os nomes da conta e do dispositivo.|
|104<br /><br />**ProtectedUserFailures-DomainController**|Motivo: Tipos de criptografia DES ou RC4 são usados para a autenticação do Kerberos e uma falha de entrada ocorre para um usuário do grupo de segurança de Usuário protegido.<br /><br />A pré-autenticação do Kerberos falhou porque os tipos de criptografia DES e RC4 não podem ser usados quando a conta é membro do grupo de segurança de Usuários protegidos.<br /><br />(AES é aceitável.)|
|303<br /><br />**ProtectedUserSuccesses-DomainController**|Motivo: Um TGT do Kerberos foi emitido com êxito para um membro do grupo de Usuários protegidos.|



## <a name="additional-resources"></a>Recursos adicionais

-   [Proteção e gerenciamento de credenciais](credentials-protection-and-management.md)

-   [As políticas de autenticação e Silos de política de autenticação](authentication-policies-and-authentication-policy-silos.md)

-   [Como configurar contas protegidas](how-to-configure-protected-accounts.md)


