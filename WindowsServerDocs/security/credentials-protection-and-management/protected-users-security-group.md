---
title: Grupo de segurança de usuários protegidos
description: Segurança do Windows Server
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f296
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: ba53c87119e798c3d3346b8fc245ffcc4e092a4d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639803"
---
# <a name="protected-users-security-group"></a>Grupo de segurança de usuários protegidos

> Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico destinado a profissionais de TI descreve o grupo de segurança do Active Directory chamado Usuários Protegidos e explica como ele funciona. Esse grupo foi introduzido nos controladores de domínio do Windows Server 2012 R2.

## <a name="overview"></a><a name="BKMK_ProtectedUsers"></a>Visão geral

Esse grupo de segurança é projetado como parte de uma estratégia para gerenciar a exposição de credenciais dentro da empresa. Os membros deste grupo possuem proteções não configuráveis automaticamente aplicadas às suas contas. A associação ao grupo Usuários protegidos visa restringir e proteger proativamente por padrão. O único método de modificar tais proteções de uma conta é removê-la do grupo de segurança.

> [!WARNING]
> As contas para serviços e computadores nunca devem ser membros do grupo usuários protegidos. Esse grupo fornece proteção incompleta mesmo assim, porque a senha ou o certificado está sempre disponível no host. A autenticação falhará com o erro \" o nome de usuário ou a senha está incorreta \" para qualquer serviço ou computador adicionado ao grupo de usuários protegidos.

Esse grupo global relacionado ao domínio dispara proteção não configurável em dispositivos e computadores host que executam o Windows Server 2012 R2 e o Windows 8.1 ou posterior para usuários em domínios com um controlador de domínio primário executando o Windows Server 2012 R2. Isso reduz significativamente a superfície de memória padrão de credenciais quando os usuários entram em computadores com essas proteções.

Para obter mais informações, consulte [como o grupo usuários protegidos funciona](#BKMK_HowItWorks) neste tópico.


## <a name="protected-users-group-requirements"></a><a name="BKMK_Requirements"></a>Requisitos do grupo de usuários protegidos
Os requisitos para fornecer proteções de dispositivo para membros do grupo de usuários protegidos incluem:

- O grupo de segurança global de Usuários protegidos é replicado para todos os controladores de domínio na conta de domínio.

- O Windows 8.1 e o Windows Server 2012 R2 adicionaram suporte por padrão. [O Microsoft Security Advisory 2871997](/security-updates/SecurityAdvisories/2016/2871997) adiciona suporte ao Windows 7, windows Server 2008 R2 e Windows Server 2012.

Os requisitos para fornecer a proteção ao controlador de domínio para membros do grupo de Usuários protegidos são:

- Os usuários devem estar em domínios que são o nível funcional de domínio do Windows Server 2012 R2 ou superior.

### <a name="adding-protected-user-global-security-group-to-down-level-domains"></a>Adicionando grupo de segurança global de usuário protegido a domínios de nível inferior

Controladores de domínio que executam um sistema operacional anterior ao Windows Server 2012 R2 podem dar suporte à adição de membros ao novo grupo de segurança de usuário protegido. Isso permite que os usuários se beneficiem das proteções de dispositivo antes de o domínio ser atualizado.

> [!Note]
> Os controladores de domínio não oferecerão suporte a proteções de domínio.

O grupo de usuários protegidos pode ser criado [transferindo a função de emulador de controlador de domínio primário (PDC)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc816944(v=ws.10)) para um controlador de domínio que executa o Windows Server 2012 R2. Depois de o objeto do grupo ser replicado para outros controladores de domínio, a função do emulador de PDC pode se hospedada em um controlador de domínio que executa uma versão anterior do Windows Server.

### <a name="protected-users-group-ad-properties"></a><a name="BKMK_ADgroup"></a>Propriedades do AD do grupo de usuários protegidos

A tabela a seguir especifica as propriedades do grupo Usuários protegidos.

|Atributo|Valor|
|-------|-----|
|SID/RID conhecido|S-1-5-21-<domain>-525|
|Type|Domínio global|
|Contêiner padrão|CN=Usuários, DC=<domain>, DC=|
|Membros padrão|Nenhum|
|Membro padrão de|Nenhum|
|Protegido por ADMINSDHOLDER?|Não|
|É seguro movê-lo para fora do contêiner padrão?|Sim|
|É seguro delegar o gerenciamento deste grupo a administradores que não são de serviço?|Não|
|Direitos de usuário padrão|Nenhum direito de usuário padrão.|

## <a name="how-protected-users-group-works"></a><a name="BKMK_HowItWorks"></a>Como o grupo usuários protegidos funciona
Essa seção explica como o grupo Usuários protegidos funciona quando:

- Conectado a um dispositivo Windows

- O domínio da conta de usuário está em um nível funcional de domínio do Windows Server 2012 R2 ou superior

### <a name="device-protections-for-signed-in-protected-users"></a>Proteções de dispositivo para usuários protegidos conectados
Quando o usuário conectado for membro do grupo de usuários protegidos, as seguintes proteções serão aplicadas:

- A delegação de credenciais (CredSSP) não armazenará em cache as credenciais de texto sem formatação do usuário mesmo quando a configuração **Permitir Delegação de credenciais padrão** política de grupo estiver habilitada.

- A partir do Windows 8.1 e do Windows Server 2012 R2, o Windows Digest não armazenará em cache as credenciais de texto sem formatação do usuário mesmo quando o Windows Digest estiver habilitado.

> [!Note]
> Depois de instalar o [Microsoft Security Advisory 2871997](/security-updates/SecurityAdvisories/2016/2871997) , o resumo do Windows continuará a armazenar as credenciais em cache até a chave do registro ser configurada. Consulte [consultoria de segurança da Microsoft: atualizar para melhorar a proteção e o gerenciamento de credenciais: 13 de maio de 2014](https://support.microsoft.com/help/2871997/microsoft-security-advisory-update-to-improve-credentials-protection-a) para obter instruções.

- O NTLM não armazenará em cache as credenciais de texto sem formatação do usuário ou a função unidirecional do NT (NTOWF).

- O Kerberos não criará mais chaves DES ou RC4. Além disso, ele não armazenará em cache as credenciais de texto sem formatação do usuário ou chaves de longo prazo depois que o TGT inicial for adquirido.

- Um verificador em cache não é criado na entrada ou no desbloqueio, portanto, não há mais suporte para entrada offline.

Depois que a conta de usuário for adicionada ao grupo de usuários protegidos, a proteção será iniciada quando o usuário entrar no dispositivo.

### <a name="domain-controller-protections-for-protected-users"></a>Proteções de controlador de domínio para usuários protegidos
As contas que são membros do grupo de usuários protegidos que se autenticam em um domínio do Windows Server 2012 R2 não podem:

- Autenticar-se com autenticação NTLM.

- Usar os tipos de criptografia DES ou RC4 na pré-autenticação do Kerberos.

- Ser delegadas com delegação restrita ou irrestrita.

- Renovar os TGTs do Kerberos além do prazo inicial de quatro horas.

As definições não configuráveis para a expiração de TGTs são estabelecidas para cada conta do grupo de Usuários protegidos. Normalmente, o controlador de domínio define o tempo de vida e renovação do TGT com base nas políticas do domínio **Tempo de vida máximo para o tíquete do usuário** e **Tempo de vida máximo para renovação de tíquete do usuário**. Para o grupo de Usuários protegidos, foram definidos 600 minutos para essas políticas de domínio.

Para mais informações, consulte [Como configurar contas protegidas](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md).

## <a name="troubleshooting"></a>Solução de problemas
Dois logs administrativos operacionais estão disponíveis para ajudar a solucionar problemas de eventos relacionados aos Usuários protegidos. Esses novos logs estão localizados em Visualizador de Eventos e são desabilitados por padrão e estão localizados em **aplicativos e serviços Logs\Microsoft\Windows\Authentication**.

|ID e log de evento|Descrição|
|----------|--------|
|104<p>**ProtectedUser-Client**|Motivo: O pacote de segurança no cliente não contém as credenciais.<p>O erro é registrado em log no computador cliente quando a conta é um membro do grupo de segurança Usuários protegidos. Este evento indica que o pacote de segurança não armazena em cache as credenciais necessárias para autenticar o servidor.<p>Exibe os nomes do pacote, usuário, domínio e servidor.|
|304<p>**ProtectedUser-Client**|Motivo: o pacote de segurança não armazena as credenciais do usuário protegido.<p>Um evento informativo é registrado no cliente para indicar que o pacote de segurança não armazena em cache as credenciais de entrada do usuário. Espera-se que o Digest (WDigest), Delegação de credenciais (CredSSP) e NTLM falhem ao entrar com credenciais para Usuários protegidos. Os aplicativos ainda funcionarão se solicitarem as credenciais.<p>Exibe os nomes do pacote, usuário e domínio.|
|100<p>**ProtectedUserFailures-DomainController**|Motivo: Uma falha de logon de NTLM que ocorre em uma conta presente no grupo de segurança Usuários protegidos.<p>Um erro é registrado em log no controlador de domínio para indicar que a autenticação NTLM falhou porque a conta era membro do grupo de segurança Usuários protegidos.<p>Exibe os nomes da conta e do dispositivo.|
|104<p>**ProtectedUserFailures-DomainController**|Motivo: Tipos de criptografia DES ou RC4 são usados para a autenticação do Kerberos e uma falha de entrada ocorre para um usuário do grupo de segurança de Usuário protegido.<p>A pré-autenticação do Kerberos falhou porque os tipos de criptografia DES e RC4 não podem ser usados quando a conta é membro do grupo de segurança de Usuários protegidos.<p>(AES é aceitável.)|
|303<p>**ProtectedUserSuccesses-DomainController**|Motivo: Um TGT do Kerberos foi emitido com êxito para um membro do grupo de Usuários protegidos.|


## <a name="additional-resources"></a>Recursos adicionais

- [Proteção e gerenciamento de credenciais](credentials-protection-and-management.md)

- [Políticas de autenticação e silos de políticas de autenticação](authentication-policies-and-authentication-policy-silos.md)

- [Como configurar contas protegidas](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md)