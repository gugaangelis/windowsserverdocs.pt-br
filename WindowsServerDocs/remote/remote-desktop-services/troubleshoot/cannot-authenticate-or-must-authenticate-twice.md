---
title: O usuário não consegue se autenticar ou deve se autenticar duas vezes
description: Solução de um problema no qual o usuário não consegue se autenticar ou deve se autenticar duas vezes ao iniciar uma conexão de área de trabalho remota.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7dbb037e335af52dacbc56c920b1776be995e753
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265928"
---
# <a name="user-cant-authenticate-or-must-authenticate-twice"></a>O usuário não consegue se autenticar ou deve se autenticar duas vezes

Este artigo aborda várias questões que podem causar problemas que afetam a autenticação do usuário.

## <a name="access-denied-restricted-type-of-logon"></a>Acesso negado, tipo de logon restrito

Nessa situação, o acesso é negado a um usuário do Windows 10 tentando se conectar a computadores Windows 10 ou Windows Server 2016, com a seguinte mensagem:

> Conexão de Área de Trabalho Remota:  
> O administrador do sistema restringiu o tipo de logon (de rede ou interativo) que você pode usar. Para obter assistência, entre em contato com o administrador do sistema ou o suporte técnico.

Esse problema ocorre quando a NLA (Autenticação no Nível da Rede) é necessária para conexões RDP e o usuário não é um membro do grupo **Usuários de Área de Trabalho Remota**. Ele também pode ocorrer se o grupo **Usuários de Área de Trabalho Remota** não foi atribuído ao direito de usuário **Acessar este computador pela rede**.

Para resolver esse problema, siga um destes procedimentos:

  - [Modifique a associação do grupo do usuário ou a atribuição de direitos de usuário](#modify-the-users-group-membership-or-user-rights-assignment).
  - Desative a NLA (não recomendado).
  - Use clientes de Área de Trabalho Remota que não executem o Windows 10. Por exemplo, clientes executando Windows 7 não apresentam esse problema.

### <a name="modify-the-users-group-membership-or-user-rights-assignment"></a>Modifique a associação do grupo do usuário ou a atribuição de direitos de usuário

Se esse problema afeta um único usuário, a solução mais simples para esse problema é adicionar o usuário ao grupo **Usuários de Área de Trabalho Remota**.

Se o usuário já for um membro desse grupo (ou se vários membros do grupo tiverem o mesmo problema), verifique a configuração de direitos de usuário no computador remoto executando Windows 10 ou Windows Server 2016.

1. Abra o GPE (Editor de Objeto de Política de Grupo) e conecte-se à política local do computador remoto.
2. Navegue até **Configuração do Computador\\Configurações do Windows\\Configurações de Segurança\\Políticas Locais\\Atribuição de Direitos do Usuário**, clique com o botão direito do mouse em **Acessar este computador da rede** e, em seguida, selecione **Propriedades**.
3. Verifique a lista de usuários e grupos para **Usuários da Área de Trabalho Remota** (ou um grupo pai).
4. Se a lista não incluir **Usuários da Área de Trabalho Remota** ou um grupo pai como **Todos**, você deverá adicioná-lo à lista. Se você tiver mais de um computador em sua implantação, use um objeto de política de grupo.  
    Por exemplo, a associação padrão para **Acessar este computador da rede** inclui **Todas as pessoas**. Se a implantação usa um objeto de política de grupo para remover **Todas as pessoas**, talvez você precise restaurar o acesso ao atualizar o objeto de política de grupo para adicionar **Usuários da Área de Trabalho Remota**.

## <a name="access-denied-a-remote-call-to-the-sam-database-has-been-denied"></a>Acesso negado, uma chamada remota para o banco de dados SAM foi negada

Esse comportamento tem mais chance de ocorrer se os controladores de domínio estão executando o Windows Server 2016 ou posterior e os usuários tentam se conectar por meio de um aplicativo de conexão personalizada. Em particular, aplicativos que acessam informações de perfil do usuário no Active Directory terão o acesso negado.

Esse comportamento resulta de uma alteração no Windows. No Windows Server 2012 R2 e versões anteriores, quando um usuário entra em uma área de trabalho remota, o RCM (Gerenciador de Conexão Remota) contata o DC (controlador de domínio) para consultar as configurações que são específicas da Área de Trabalho Remota no objeto de usuário nos AD DS (Active Directory Domain Services). Essas informações são exibidas na guia Perfil de Serviços de Área de Trabalho Remota das propriedades do objeto de um usuário no snap-in do MMC de Usuários e Computadores do Active Directory.

No Windows Server 2016 e versões posteriores, o RCM não consulta mais o objeto do usuário no AD DS. Se precisar que o RCM consulte o AD DS porque está usando os atributos de Serviços de Área de Trabalho Remota, você deverá habilitar manualmente a consulta.

> [!IMPORTANT]  
> Siga as etapas nesta seção com cuidado. Problemas sérios podem ocorrer se você modificar o Registro incorretamente. Antes de modificá-lo, [faça backup do Registro para a restauração](https://support.microsoft.com/help/322756) em caso de problemas.

Para habilitar o comportamento herdado do RCM em um servidor Host da Sessão da Área de Trabalho Remota, configure as entradas do Registro a seguir e, em seguida, reinicie o serviço **Serviços de Área de Trabalho Remota**:  
  - **HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Policies\\Microsoft\\Windows NT\\Terminal Services**
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server\\WinStations\\\<Nome da Winstation\>\\**  
      - Nome: **fQueryUserConfigFromDC**
      - Tipo: **Reg\_DWORD**
      - Valor: **1** (Decimal)

Para habilitar o comportamento herdado do RCM em um servidor que não seja o Host da Sessão da Área de Trabalho Remota, configure essas entradas do Registro e a entrada do Registro adicional a seguir (e depois reinicie o serviço):
  - **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Control\\Terminal Server**

Para obter mais informações sobre esse comportamento, confira o KB 3200967 [Alterações ao Gerenciador de Conexões Remotas no Windows Server](https://support.microsoft.com/help/3200967/changes-to-remote-connection-manager-in-windows-server).

## <a name="user-cant-sign-in-using-a-smart-card"></a>O usuário não consegue entrar usando um cartão inteligente

Esta seção aborda três cenários comuns em que um usuário não consegue entrar em uma área de trabalho remota usando um cartão inteligente.

### <a name="cant-sign-in-with-a-smart-card-in-a-branch-office-with-a-read-only-domain-controller-rodc"></a>Não é possível entrar com um cartão inteligente em uma filial com um RODC (controlador de domínio somente leitura)

Esse problema ocorre em implantações que incluem um servidor RDSH em um site de filial que usa um RODC. O servidor de RDSH é hospedado no domínio raiz. Os usuários do site de filial pertencem a um domínio filho e usam cartões inteligentes para autenticação. O RODC está configurado para armazenar senhas de usuário no cache (o RODC pertence ao **Grupo de Replicação de Senha RODC Permitido**). Quando os usuários tentarem entrar em sessões no servidor RDSH, eles receberão mensagens como "A tentativa de logon é inválida. Isso é devido a uma informação de autenticação ou nome de usuário inválida".

Esse problema é causado pela maneira como o DC raiz e o RDOC gerenciam a criptografia de credenciais de usuário. O DC raiz usa uma chave de criptografia para criptografar as credenciais e o RODC fornece ao cliente a chave de descriptografia. Quando um usuário recebe o erro “inválido”, isso significa que duas chaves não correspondem.

Para solucionar esse problema, experimente seguir um destes procedimentos:

- Altere sua topologia de DC, seja desativando o cache de senhas no RODC ou implantando um DC gravável no site de filial.
- Migre o servidor RDSH para o mesmo domínio filho que os usuários.
- Permita que os usuários entrem sem cartões inteligentes.

Saiba que todas essas soluções exigem comprometimentos no nível do desempenho ou da segurança.

### <a name="user-cant-sign-in-to-a-windows-server-2008-sp2-computer-using-a-smart-card"></a>O usuário não consegue entrar em um computador com Windows Server 2008 SP2 usando um cartão inteligente

Esse problema ocorre quando os usuários entram em um computador executando Windows Server 2008 SP2 que foi atualizado com KB4093227 (2018.4B). Quando os usuários tentam entrar usando um cartão inteligente, o acesso é negado a eles com mensagens como "Nenhum certificado válido encontrado. Verifique se o cartão está inserido corretamente e se encaixa perfeitamente." Ao mesmo tempo, o computador com Windows Server registra o evento de aplicativo "Ocorreu um erro ao recuperar um certificado digital do cartão inteligente inserido. Assinatura inválida."

Para resolver esse problema, atualize o computador com Windows Server com o relançamento 2018.06 B do KB 4093227, [Descrição da atualização de segurança para a vulnerabilidade de negação de serviço do protocolo RDP no Windows Server 2008: 10 de abril de 2018](https://support.microsoft.com/help/4093227/security-update-for-vulnerabilities-in-windows-server-2008).

### <a name="cant-stay-signed-in-with-a-smart-card-and-remote-desktop-services-service-hangs"></a>Não é possível permanecer conectado com um cartão inteligente e o serviço de Serviços de Área de Trabalho Remota trava

Esse problema ocorre quando os usuários entram em um computador executando Windows ou Windows Server que foi atualizado com KB 4056446. Primeiro, o usuário poderá conseguir entrar no sistema usando um cartão inteligente, mas, em seguida, receberá uma mensagem de erro "SCARD\_E\_NO\_SERVICE". O computador remoto poderá parar de responder.

Para contornar esse problema, reinicie o computador remoto.

Para resolver esse problema, atualize o computador remoto com a correção apropriada:

  - Windows Server 2008 SP2: KB 4090928, [O Windows vaza identificadores no processo lsm.exe e aplicativos de cartão inteligente podem exibir erros "SCARD\_E\_NO\_SERVICE"](https://support.microsoft.com/help/4090928/scard-e-no-service-errors-when-windows-leaks-handles-in-the-lsm-exe)
  - Windows Server 2012 R2: KB 4103724, [17 de maio de 2018 – KB4103724 (versão prévia do pacote cumulativo de atualizações mensal)](https://support.microsoft.com/help/4103724/windows-81-update-kb4103724)
  - Windows Server 2016 e Windows 10, versão 1607: KB 4103720, [17 de maio de 2018 – KB4103720 (Build do sistema operacional 14393.2273)](https://support.microsoft.com/help/4103720/windows-10-update-kb4103720)

## <a name="if-the-remote-pc-is-locked-the-user-needs-to-enter-a-password-twice"></a>Se o computador remoto estiver bloqueado, o usuário precisará inserir uma senha duas vezes

Esse problema pode ocorrer quando um usuário tenta se conectar a uma área de trabalho remota que está executando o Windows 10 versão 1709 em uma implantação em que as conexões RDP não exigem NLA. Sob essas condições, se a Área de Trabalho Remota tiver sido bloqueada, o usuário precisará inserir suas credenciais duas vezes ao se conectar.

Para resolver esse problema, atualize o computador que executa o Windows 10 versão 1709 com o KB 4343893 de [30 de agosto de 2018 – KB4343893 (SO Build 16299.637)](https://support.microsoft.com/help/4343893/windows-10-update-kb4343893).

## <a name="user-cant-sign-in-and-receives-authentication-error-and-credssp-encryption-oracle-remediation-messages"></a>Um usuário não consegue entrar e recebe as mensagens "erro de autenticação" e "correção de Oráculo de criptografia CredSSP"

Quando os usuários tentam entrar usando qualquer versão do Windows, do Windows Vista SP2 e versões posteriores ou do Windows Server 2008 SP2 e versões posteriores, o acesso é negado a eles, que recebem mensagens como as seguintes:

```
An authentication error has occurred. The function requested is not supported.
...
This could be due to CredSSP encryption oracle remediation
...
```

"Correção de Oráculo de Criptografia CredSSP" refere-se a um conjunto de atualizações de segurança lançado em março, abril e maio de 2018. O CredSSP é um provedor de autenticação que processa solicitações de autenticação para outros aplicativos. A atualização "3B" de 13 de março de 2018 e as atualizações subsequentes resolveram uma exploração em que um invasor podia retransmitir credenciais do usuário para executar o código no sistema de destino.

As atualizações inicias adicionaram suporte para um novo objeto de política de grupo, Correção do Oráculo de Criptografia, que tem as seguintes configurações possíveis:

  - Vulnerável: Aplicativos cliente que usam o CredSSP podem reverter para versões não seguras, mas esse comportamento expõe as Áreas de Trabalho Remotas a ataques. Serviços que usam o CredSSP aceitam clientes que não foram atualizados.
  - Atenuado: aplicativos cliente que usam o CredSSP não podem reverter para versões inseguras, mas os serviços que usam o CredSSP aceitam clientes que não foram atualizados.
  - Forçar Clientes Atualizados: aplicativos cliente que usam o CredSSP não podem reverter para versões inseguras e os serviços que usam o CredSSP não aceitam clientes sem patch. 
    > [!NOTE]
    > Essa configuração não deve ser implantada até que todos os hosts remotos sejam compatíveis com a versão mais recente.

A atualização de 8 de maio de 2018 alterou a configuração padrão Correção do Oráculo de Criptografia de Vulnerável para Atenuado. Com essa alteração em vigor, os clientes da Área de Trabalho Remota que têm as atualizações não conseguem se conectar a servidores que não as têm (nem a servidores atualizados que não foram reiniciados). Para saber mais sobre atualizações CredSSP, confira [KB 4093492](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018).

Para resolver esse problema, atualize e reinicie todos os sistemas. Para obter uma lista completa de atualizações e obter mais informações sobre vulnerabilidades, confira [CVE-2018-0886 | Vulnerabilidade de execução de código remoto de CredSSP](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2018-0886).

Para contornar esse problema até as atualizações serem concluídas, confira o KB 4093492 para os tipos permitidos de conexões. Se não há nenhuma alternativa viável, você pode considerar um dos seguintes métodos:

- Para computadores cliente afetados, defina a política de Correção do Oráculo de Criptografia de volta como **Vulnerável**.
- Modifique as políticas a seguir na pasta de política de grupo **Configuração do Computador\\Modelos Administrativos\\Componentes do Windows\\Serviços de Área de Trabalho Remota\\Host da Sessão da Área de Trabalho Remota\\Segurança**:  
  - **Exigir o uso de camada de segurança específica para conexões remotas (RDP)** : defina para **Habilitado** e selecione **RDP**.
  - **Exigir autenticação de usuário para conexões remotas usando a Autenticação no Nível da Rede**: defina para **Desabilitado**.
    > [!IMPORTANT]  
    > A alteração dessas políticas de grupo reduz a segurança da implantação. Recomendamos que você só as use temporariamente, se precisar.

Para obter mais informações sobre como trabalhar com a política de grupo, confira [Modificar um GPO de bloqueio](rdp-error-general-troubleshooting.md#modifying-a-blocking-gpo).

## <a name="after-you-update-client-computers-some-users-need-to-sign-in-twice"></a>Depois de você atualizar os computadores cliente, alguns usuários precisam entrar duas vezes

Quando os usuários entram na Área de Trabalho Remota usando um computador que executa o Windows 7 ou o Windows 10, versão 1709, eles imediatamente veem um segundo prompt de conexão. Esse problema acontecerá se o computador cliente tiver as seguintes atualizações:

  - Windows 7: KB 4103718, [8 de maio de 2018 – KB4103718 (pacote cumulativo de atualizações mensal)](https://support.microsoft.com/help/4103718/windows-7-update-kb4103718)
  - Windows 10 1709: KB 4103727, [8 de maio de 2018 – KB4103727 (Build do sistema operacional 16299.431)](https://support.microsoft.com/help/4103727/windows-10-update-kb4103727)

Para resolver esse problema, verifique se os computadores com os quais os usuários desejam se conectar (bem como os servidores RDSH ou RDVI) estão totalmente atualizados até junho de 2018. Isso inclui as seguintes atualizações:

  - Windows Server 2016: KB 4284880, [12 de junho de 2018 – KB4284880 (Build do sistema operacional 14393.2312)](https://support.microsoft.com/help/4284880/windows-10-update-kb4284880)
  - Windows Server 2012 R2: KB 4284815, [12 de junho de 2018 – KB4284815 (pacote cumulativo de atualizações mensal)](https://support.microsoft.com/help/4284815/windows-81-update-kb4284815)
  - Windows Server 2012: KB 4284855, [12 de junho de 2018 – KB4284855 (pacote cumulativo de atualizações mensal)](https://support.microsoft.com/help/4284855/windows-server-2012-update-kb4284855)
  - Windows Server 2008 R2: KB 4284826, [12 de junho de 2018 – KB4284826 (pacote cumulativo de atualizações mensal)](https://support.microsoft.com/help/4284826/windows-7-update-kb4284826)
  - Windows Server 2008 SP2: KB4056564, [Descrição da atualização de segurança para a vulnerabilidade de execução de código remoto do CredSSP no Windows Server 2008, Windows Embedded POSReady 2009 e Windows Embedded Standard 2009: 13 de março de 2018](https://support.microsoft.com/help/4056564/security-update-for-vulnerabilities-in-windows-server-2008)

## <a name="users-are-denied-access-on-a-deployment-that-uses-remote-credential-guard-with-multiple-rd-connection-brokers"></a>O acesso é negado aos usuários em uma implantação que usa o Credential Guard remoto com vários agentes de Conexão de Área de Trabalho Remota

Esse problema ocorre em implantações de alta disponibilidade que usam dois ou mais agentes de Conexão de Área de Trabalho Remota se o Credential Guard remoto do Windows Defender está em uso. Os usuários não conseguem entrar em áreas de trabalho remotas.

Esse problema ocorre porque o Credential Guard remoto usa o Kerberos para autenticação e restringe o NTLM. No entanto, em uma configuração de alta disponibilidade com balanceamento de carga, os Agentes de Conexão de Área de Trabalho Remota não podem dar suporte a operações do Kerberos.

Se você precisar usar uma configuração de alta disponibilidade com agentes de Conexão de Área de Trabalho Remota com balanceamento de carga, poderá contornar esse problema desabilitando o Credential Guard remoto. Para obter mais informações sobre como gerenciar o Credential Guard remoto do Windows Defender, confira [Proteger credenciais da Área de Trabalho Remota com o Credential Guard remoto do Windows Defender](https://docs.microsoft.com/windows/security/identity-protection/remote-credential-guard#enable-windows-defender-remote-credential-guard).
