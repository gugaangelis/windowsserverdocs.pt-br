---
title: Solucionando problemas do AD FS - autenticação integrada com o Windows
description: Este documento descreve como solucionar problemas de autenticação integrada do windows
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9703a8652b0e0bbafe48858cbfbcc8aa9aa31ef8
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812051"
---
# <a name="ad-fs-troubleshooting---integrated-windows-authentication"></a>Solucionando problemas do AD FS - autenticação integrada com o Windows
Autenticação integrada do Windows permite que os usuários entrem com suas credenciais do Windows e a experiência de logon único (SSO), usando Kerberos ou NTLM.

## <a name="reason-integrated-windows-authentication-fails"></a>Falha de autenticação integrada do windows de motivo
Há três principal motivo por que a autenticação integrada do windows falhará. São eles:
    - Configuração incorreta da entidade de serviço Name(SPN)
    - Token de associação de canal
    - Configuração do Internet Explorer

## <a name="spn-misconfiguration"></a>Erro de configuração de SPN
Um nome de entidade de serviço (SPN) é um identificador exclusivo de uma instância de serviço. Os SPNs são usados pela autenticação do Kerberos para associar uma instância de serviço com uma conta de logon do serviço. Isso permite que um aplicativo cliente solicitar que o serviço autentique uma conta, mesmo se o cliente não tem o nome da conta.

Um exemplo de um como um SPN é usado com o AD FS é da seguinte maneira:
1. Um navegador da web consultará o Active Directory para determinar qual conta de serviço está em execução sts.contoso.com
2. Active Directory informa ao navegador que é a conta de serviço do AD FS.
3. O navegador obterá um tíquete Kerberos para a conta de serviço do AD FS.

Se a conta de serviço do AD FS tiver um configurado incorretamente ou o SPN errado, em seguida, isso pode causar problemas.  Examinar os rastreamentos de rede, você poderá ver erros como erro KRB: KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN.

Usando rastreamentos de rede (como Wireshark) você pode determinar qual SPN para o navegador está tentando resolver e, em seguida, usando a linha de comando de ferramentas, setspn – Q <spn>, você pode fazer uma pesquisa em que o SPN.  Ele não pode ser encontrado ou ele pode ser atribuído a outra conta diferente da conta de serviço do AD FS.

![Integrado](media/ad-fs-tshoot-iwa/iwa3.png)

Você pode verificar o SPN, observando as propriedades da conta de serviço do AD FS.

![Integrado](media/ad-fs-tshoot-iwa/iwa1.png)

## <a name="channel-binding-token"></a>Token de associação de canal
Atualmente, quando um aplicativo cliente se autentica no servidor usando Kerberos, Digest ou NTLM usando HTTPS, primeiro é estabelecer um canal de segurança de nível de transporte (TLS) e a autenticação é feita usando este canal. 

O Token de associação de canal é uma propriedade do canal externo protegido por TLS e é usado para associar o canal externo para uma conversa sobre o canal interno autenticado pelo cliente.

Se não houver um ataque "man-in-the-middle" que ocorrem e são de crypting e criptografar novamente o tráfego SSL e a chave não corresponde à.  O AD FS determinará o que há algo no meio entre ele e o r de navegação da web.  Isso fará com que a autenticação Kerberos falhe e o usuário será solicitado com uma caixa de diálogo 401 em vez de uma experiência de SSO.

Isso pode ser causado por:
 - qualquer coisa entre o navegador e o AD FS
 - Fiddler
 - Proxies reversos executando a ponte SSL

Por padrão, o AD FS tem isso definido como "Permitir".  Você pode alterar essa configuração usando o cmdlet do PowerShell `Set-ADFSProperties -ExtendProtectionTokenCheck`

Para obter mais informações sobre isso, consulte [práticas recomendadas para proteger planejamento e implantação do AD FS](../../ad-fs/design/best-practices-for-secure-planning-and-deployment-of-ad-fs.md).

## <a name="internet-explorer-configuration"></a>Configuração do Internet Explorer
Por padrão, a Internet explorer terão da seguinte maneira:

1. Do Internet explorer recebe uma resposta 401 do AD FS com a palavra NEGOTIATE no cabeçalho.
2. Isso informa ao navegador da web para obter um tíquete Kerberos ou NTLM para enviar de volta para o AD FS.
3. Por padrão o IE tentará fazer essa SPNEGO () sem interação do usuário, se a palavra NEGOTIATE está no cabeçalho.  Ele só funcionará para sites de intranet.

Há 2 coisas principais que podem impedir que isso happeing.
   - Habilitar a autenticação integrada do Windows não está marcada nas propriedades do IE.  Ela está localizada em Opções da Internet -> Avançado -> segurança.
   
   ![Integrado](media/ad-fs-tshoot-iwa/iwa4.png)
   
   - As zonas de segurança não estão configuradas corretamente
       - FQDNs não estão na zona da intranet
       - URL do AD FS não está na zona da intranet.

      ![Integrado](media/ad-fs-tshoot-iwa/iwa5.png)
## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)
