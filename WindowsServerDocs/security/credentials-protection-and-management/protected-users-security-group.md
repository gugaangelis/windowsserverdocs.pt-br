---
title: "Grupo de segurança de usuários protegido"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="protected-users-security-group"></a>Grupo de segurança de usuários protegido

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico para o profissional de TI descreve o grupo de segurança do Active Directory usuários protegidos e explica como ele funciona. Esse grupo foi introduzido no Windows Server 2012 R2 controladores de domínio.

## <a name="BKMK_ProtectedUsers"></a>Visão geral

Esse grupo de segurança foi projetado como parte de uma estratégia para gerenciar a exposição de credenciais da empresa. Os membros deste grupo têm automaticamente não configuráveis proteções aplicadas às suas contas. Associação ao grupo usuários protegido deve ser restritivas e proativamente seguro por padrão. É o único método para modificar essas proteções de uma conta remover a conta do grupo de segurança.

> [!WARNING]
> Contas para serviços e computadores nunca devem ser membros do grupo usuários protegido. Esse grupo faria oferece proteção incompleta mesmo assim porque a senha ou certificado sempre está disponível no host. Autenticação falhará com o nome de usuário do erro \"the ou senha é incorrect\" para qualquer serviço ou um computador que é adicionado ao grupo usuários protegido.

Esse grupo relacionados ao domínio, global dispara proteção não configuráveis em dispositivos e computadores host que executam o Windows Server 2012 R2 e Windows 8.1 ou posterior para os usuários em domínios com um controlador de domínio primário executando o Windows Server 2012 R2. Isso reduz significativamente o volume de memória padrão de credenciais, quando os usuários entrar em computadores com essas proteções.

Para obter mais informações, consulte [como os usuários protegidos agrupar funciona](#BKMK_HowItWorks) neste tópico.



## <a name="BKMK_Requirements"></a>Requisitos de grupo de usuários protegidos
Requisitos para fornecer proteções de dispositivo para membros do grupo usuários protegida incluem:

- O grupo de segurança global de usuários protegido é replicado para todos os controladores de domínio no domínio de conta.

- Windows 8.1 e Windows Server 2012 R2 adicionou suporte por padrão. [Microsoft Security Advisory 2871997](https://technet.microsoft.com/library/security/2871997) adiciona o suporte ao Windows 7, Windows Server 2008 R2 e Windows Server 2012.

Requisitos para fornecer proteção de controlador de domínio para membros do grupo usuários protegida incluem:

- Os usuários devem estar em domínios que são Windows Server 2012 R2 ou superior nível funcional do domínio.

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>Adicionar grupo de segurança global protegido por usuário para domínios de nível inferior

Controladores de domínio que executam sistemas operacionais anteriores ao Windows Server 2012 R2 podem dar suporte a adicionar membros para o novo grupo de segurança de usuário protegido. Isso permite que os usuários se beneficiar proteções de dispositivo antes que o domínio seja atualizado. 

> [!Note]
> Os controladores de domínio não suportará proteções de domínio. 

Grupo de usuários protegido pode ser criado por [transferindo a função de emulador do domínio primário (PDC) do controlador](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx) para um controlador de domínio que executa o Windows Server 2012 R2. Depois que esse objeto de grupo é replicado para outros controladores de domínio, a função de emulador do PDC pode ser hospedada em um controlador de domínio que executa uma versão anterior do Windows Server.

### <a name="BKMK_ADgroup"></a>Propriedades de grupo AD usuários protegidas

A tabela a seguir especifica as propriedades do grupo usuários protegido.

|Atributo|Valor|
|-------|-----|
|SID/RID conhecido|S-1-5-21 -<domain>-525|
|Tipo|Domínio Global|
|Contêiner padrão|CN = usuários, DC =<domain>, DC =|
|Membros padrão|None|
|Membro padrão de|None|
|Protegidos por ADMINSDHOLDER?|Não|
|Seguras para deixarem contêiner padrão?|Sim|
|Delegar o gerenciamento desse grupo de administradores de serviço não é seguro?|Não|
|Direitos de usuário padrão|Não há direitos de usuário padrão|

## <a name="BKMK_HowItWorks"></a>Como funciona o grupo usuários protegido
Esta seção explica como o grupo usuários protegido funciona quando:

-   Logon em um dispositivo Windows

-   Domínio da conta de usuário está em um Windows Server 2012 R2 ou superior nível funcional do domínio

### <a name="device-protections-for-signed-in-protected-users"></a>Proteções de dispositivo para logon protegido por usuários
Quando o usuário conectado é um membro do grupo usuários protegido as seguintes proteções são aplicadas:

-   Credenciais será delegação (CredSSP) armazena em cache as credenciais do usuário texto sem formatação mesmo quando o **permitir delegação de credenciais padrão** configuração de política de grupo está habilitada.

-   A partir do Windows 8.1 e Windows Server 2012 R2, Windows Digest irá não armazenar em cache as credenciais do usuário texto sem formatação até mesmo quando o resumo do Windows está habilitado.

> [!Note]
> Depois de instalar [Microsoft Security Advisory 2871997](https://technet.microsoft.com/library/security/2871997) Digest Windows continuará em cache as credenciais até que a chave do registro está configurada. Consulte [Microsoft Security Advisory: atualização para melhorar a proteção e gerenciamento de credenciais: 13 de maio de 2014](https://support.microsoft.com/en-us/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a) para obter instruções.

-   NTLM não armazenará em cache as credenciais do usuário texto sem formatação ou função unidirecional NT (NTOWF).

-   Kerberos não irá criar chaves DES ou RC4. Também ele irá não armazenar em cache credenciais em texto simples ou chaves de longo prazo do usuário depois que o TGT inicial é adquirido.

-   Um verificador armazenado em cache não é criado quando você entrar na rede ou desbloquear, portanto, entrar offline não é mais compatível.

Depois que a conta de usuário é adicionada ao grupo usuários protegido, proteção será iniciada quando o usuário entra no dispositivo.

### <a name="domain-controller-protections-for-protected-users"></a>Proteções de controlador de domínio para que os usuários protegido
Contas que são membros do grupo usuários protegido que se autenticar em um domínio do Windows Server 2012 R2 não são capazes de:

-   Autenticar com a autenticação NTLM.

-   Use tipos de criptografia DES ou RC4 na pré-autenticação Kerberos.

-   Ser recebido com a delegação irrestrita ou restrita.

-   Renove o TGTs Kerberos além do tempo de vida de quatro horas inicial.

Configurações não são configuráveis para a expiração TGTs são estabelecidas para cada conta no grupo de usuários protegido. Normalmente, o controlador de domínio define o tempo de vida de TGTs e renovação, com base em políticas de domínio, **tempo de vida máximo para o tíquete de usuário de** e **vida útil máxima para renovação de tíquete de usuário**. Para o grupo de usuários protegido, 600 minutos é definido para essas políticas de domínio.

Para obter mais informações, consulte [como configurar contas protegido](how-to-configure-protected-accounts.md).

## <a name="troubleshooting"></a>Solução de problemas
Dois logs administrativos operacionais estão disponíveis para ajudar a solucionar problemas de eventos que estão relacionados aos usuários protegido. Esses novos logs estão localizados no Visualizador de eventos são desabilitados por padrão e estão localizados em **aplicativos e serviços Logs\Microsoft\Windows\Microsoft\Authentication**.

|Log e ID de evento|Descrição|
|----------|--------|
|104<br /><br />**ProtectedUser-Client**|Motivo: O pacote de segurança no cliente não contém as credenciais.<br /><br />O erro é registrado no computador cliente quando a conta é um membro do grupo de segurança usuários protegido. Esse evento indica que o pacote de segurança não armazena em cache as credenciais que são necessárias para realizar autenticação no servidor.<br /><br />Exibe o nome do pacote, nome de usuário, nome de domínio e nome do servidor.|
|304<br /><br />**ProtectedUser-Client**|Motivo: O pacote de segurança não armazena as credenciais do usuário protegido.<br /><br />Um evento informativo é registrado no cliente para indicar que o pacote de segurança não armazena em cache as credenciais do usuário entrar. Espera-se que Digest (WDigest), a delegação de credenciais (CredSSP) e NTLM falham com credenciais de logon para os usuários protegido. Aplicativos ainda podem acontecer se eles solicitar credenciais.<br /><br />Exibe o nome do pacote, o nome de usuário e o nome de domínio.|
|100<br /><br />**ProtectedUserFailures-DomainController**|Motivo: Ocorre uma falha de entrar NTLM para uma conta que esteja no grupo de segurança de usuários protegidos.<br /><br />Um erro é registrado no controlador de domínio para indicar que a autenticação NTLM falhou porque a conta foi um membro do grupo de segurança usuários protegido.<br /><br />Exibe o nome da conta e o nome do dispositivo.|
|104<br /><br />**ProtectedUserFailures-DomainController**|Motivo: Tipos de criptografia DES ou RC4 são usados para autenticação Kerberos e ocorrer uma falha de entrada para um usuário no grupo de segurança protegido por usuário.<br /><br />Falha na pré-autenticação Kerberos porque os tipos de criptografia DES e RC4 não podem ser usados quando a conta é um membro do grupo de segurança usuários protegido.<br /><br />(AES é aceitável.)|
|303<br /><br />**ProtectedUserSuccesses-DomainController**|Motivo: Um Kerberos tíquete-concessão-tíquete (TGT) com êxito foi emitido para um membro do grupo de usuários protegido.|



## <a name="additional-resources"></a>Recursos adicionais

-   [Gerenciamento e proteção de credenciais](credentials-protection-and-management.md)

-   [Políticas de autenticação e Silos de política de autenticação](authentication-policies-and-authentication-policy-silos.md)

-   [Como configurar contas protegidas](how-to-configure-protected-accounts.md)


