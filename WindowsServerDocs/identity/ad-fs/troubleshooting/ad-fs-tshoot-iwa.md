---
title: Solução de problemas AD FS-autenticação integrada do Windows
description: Este documento descreve como solucionar problemas de autenticação integrada do Windows
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.openlocfilehash: 4c1823e1cbfc58e50c7231293b846c31480e01a6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954152"
---
# <a name="ad-fs-troubleshooting---integrated-windows-authentication"></a>Solução de problemas AD FS-autenticação integrada do Windows
A autenticação integrada do Windows permite que os usuários façam logon com suas credenciais do Windows e experimentem SSO (logon único) usando Kerberos ou NTLM.

## <a name="reason-integrated-windows-authentication-fails"></a>Motivo da falha de autenticação integrada do Windows
Há três razões principais pelas quais a autenticação integrada do Windows falhará. Eles são:
    - Nome da entidade de serviço (SPN) configuração incorreta
    - Token de associação de canal
    - Configuração do Internet Explorer

## <a name="spn-misconfiguration"></a>Inconfiguração de SPN
Um SPN (nome da entidade de serviço) é um identificador exclusivo de uma instância de serviço. Os SPNs são usados pela autenticação Kerberos para associar uma instância de serviço a uma conta de logon de serviço. Isso permite que um aplicativo cliente solicite que o serviço autentique uma conta mesmo que o cliente não tenha o nome da conta.

Um exemplo de como um SPN é usado com AD FS é o seguinte:
1. Um navegador da Web consulta Active Directory para determinar qual conta de serviço está executando sts.contoso.com
2. Active Directory informa ao navegador que é a conta de serviço do AD FS.
3. O navegador receberá um tíquete Kerberos para a conta de serviço AD FS.

Se a conta de serviço do AD FS tiver um SPN mal configurado ou incorreto, isso poderá causar problemas.  Observando os rastreamentos de rede, você poderá ver erros como o erro KRB: KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN.

Usando os rastreamentos de rede (como o Wireshark), você pode determinar qual SPN o navegador está tentando resolver e, em seguida, usar a ferramenta de linha de comando, SetSPN-Q <spn> , você pode fazer uma pesquisa nesse SPN.  Ele pode não ser encontrado ou pode ser atribuído a outra conta que não seja a conta de serviço AD FS.

![integrado](media/ad-fs-tshoot-iwa/iwa3.png)

Você pode verificar o SPN examinando as propriedades da conta de serviço AD FS.

![integrado](media/ad-fs-tshoot-iwa/iwa1.png)

## <a name="channel-binding-token"></a>Token de associação de canal
Atualmente, quando um aplicativo cliente se autentica no servidor usando Kerberos, Digest ou NTLM usando HTTPS, um canal de TLS (segurança de nível de transporte) é estabelecido primeiro e a autenticação ocorre usando esse canal.

O token de associação de canal é uma propriedade do canal externo protegido por TLS e é usado para associar o canal externo a uma conversa no canal interno autenticado pelo cliente.

Se houver um ataque "Man-in-the-middle" que ocorre e ele estiver descriptografando e criptografando novamente o tráfego SSL, a chave não será correspondente.  AD FS determinará que há algo localizado no meio entre o r de navegação na Web e o mesmo.  Isso fará com que a autenticação Kerberos falhe e o usuário será solicitado com uma caixa de diálogo 401 em vez de uma experiência de SSO.

Isso pode ser causado por:
 - qualquer coisa situada entre o navegador e o AD FS
 - Fiddler
 - Proxies reversos executando ponte SSL

Por padrão, AD FS tem isso definido como "permitir".  Você pode alterar essa configuração usando o cmdlt do PowerShell`Set-ADFSProperties -ExtendProtectionTokenCheck`

Para obter mais informações sobre isso [, consulte práticas recomendadas para o planejamento e a implantação seguros de AD FS](../../ad-fs/design/best-practices-for-secure-planning-and-deployment-of-ad-fs.md).

## <a name="internet-explorer-configuration"></a>Configuração do Internet Explorer
Por padrão, o Internet Explorer terá o seguinte modo:

1. O Internet Explorer receberá uma resposta de 401 de AD FS com a palavra NEGOTIAte no cabeçalho.
2. Isso diz ao navegador da Web para obter um tíquete Kerberos ou NTLM para enviar de volta para o AD FS.
3. Por padrão, o IE tentará fazer isso (SPNEGO) sem interação do usuário se a palavra NEGOTIAte estiver no cabeçalho.  Ele só funcionará para sites de intranet.

Há duas coisas principais que podem impedir isso de happeing.
   - Habilitar a autenticação integrada do Windows não é verificado nas propriedades do IE.  Isso está localizado em opções da Internet-> segurança avançada->.

   ![integrado](media/ad-fs-tshoot-iwa/iwa4.png)

   - As zonas de segurança não estão configuradas corretamente
       - Os FQDNs não estão na zona da intranet
       - A URL do AD FS não está na zona da intranet.

      ![integrado](media/ad-fs-tshoot-iwa/iwa5.png)
## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)
