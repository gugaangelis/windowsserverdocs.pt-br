---
title: What's New in Kerberos Authentication
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: a0916abf1076b5f791a856f0c85f54ad17f6d64c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403480"
---
# <a name="whats-new-in-kerberos-authentication"></a>What's New in Kerberos Authentication

>Aplica-se a: Windows Server 2016 e Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>Suporte de KDC para autenticação de cliente baseada em confiança de chave pública

A partir do Windows Server 2016, o KDCs dá suporte a uma maneira de mapeamento de chave pública. Se a chave pública for provisionada para uma conta, o KDC oferecerá suporte ao Kerberos PKInit explicitamente usando essa chave. Como não há nenhuma validação de certificado, há suporte para certificados autoassinados e a garantia de mecanismo de autenticação não é suportada.

A relação de confiança de chave é preferida quando configurada para uma conta, independentemente da configuração UseSubjectAltName.

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Suporte a cliente Kerberos e KDC para a extensão de atualização do RFC 8070 PKInit

A partir do Windows 10, versão 1607 e Windows Server 2016, os clientes Kerberos tentam a [extensão de atualização do RFC 8070 PKInit](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/) para os logons baseados em chave pública. 

A partir do Windows Server 2016, o KDCs pode dar suporte à extensão de atualização PKInit. Por padrão, KDCs não oferecem a extensão de atualização PKInit. Para habilitá-lo, use a configuração de política de modelo administrativo do novo suporte do KDC para a extensão de atualização PKInit em todos os DCs no domínio. Quando configurado, as seguintes opções têm suporte quando o domínio é o nível funcional de domínio do Windows Server 2016 (DFL):

- **Desabilitado**: O KDC nunca oferece a extensão de atualização PKInit e aceita solicitações de autenticação válidas sem verificar a atualização. Os usuários nunca receberão o novo SID de identidade de chave pública.
- **Com suporte**: A extensão de atualização de PKInit tem suporte na solicitação. Os clientes Kerberos autenticados com êxito com a extensão de atualização PKInit recebem o novo SID de identidade de chave pública.
- **Necessário**: A extensão de atualização PKInit é necessária para a autenticação bem-sucedida. Os clientes Kerberos que não suportam a extensão de atualização PKInit sempre falharão ao usar credenciais de chave pública.

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Suporte a dispositivos ingressados no domínio para autenticação usando chave pública

A partir do Windows 10 versão 1507 e do Windows Server 2016, se um dispositivo ingressado no domínio for capaz de registrar sua chave pública vinculada com um controlador de domínio do Windows Server 2016 (DC), o dispositivo poderá autenticar com a chave pública usando a autenticação Kerberos para um Windows Server 2016 DC. Para obter mais informações, consulte [autenticação de chave pública do dispositivo ingressado no domínio](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Os clientes Kerberos permitem nomes de host de endereço IPv4 e IPv6 em SPNs (nome da entidade de serviço)

A partir do Windows 10 versão 1507 e do Windows Server 2016, os clientes Kerberos podem ser configurados para dar suporte a nomes de host IPv4 e IPv6 em SPNs. 

Caminho do registro:

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

Para configurar o suporte para nomes de host de endereço IP em SPNs, crie uma entrada TryIPSPN. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. Se não estiver configurado, os nomes de host de endereço IP não serão tentados.

Se o SPN estiver registrado no Active Directory, a autenticação terá sucesso com o Kerberos. 

Para obter mais informações, confira o documento [Configurando o Kerberos para endereços IP](configuring-kerberos-over-ip.md).

## <a name="kdc-support-for-key-trust-account-mapping"></a>Suporte de KDC para mapeamento de conta de confiança de chave

A partir do Windows Server 2016, os controladores de domínio têm suporte para mapeamento de conta de confiança de chave, bem como fallback para AltSecID existentes e UPN (nome principal de usuário) no comportamento de SAN. Quando UseSubjectAltName é definido como:

- 0: É necessário um mapeamento explícito. Em seguida, deve haver:
    - Confiança de chave (novidade com o Windows Server 2016)
    - ExplicitAltSecID
- 1: O mapeamento implícito é permitido (padrão):
    1. Se a confiança de chave estiver configurada para a conta, ela será usada para mapeamento (novo com o Windows Server 2016).
    2. Se não houver nenhum UPN na SAN, a AltSecID será tentada para o mapeamento.
    3. Se houver um UPN na SAN, a UPN será tentada para o mapeamento.

## <a name="see-also"></a>Consulte também

- [Visão geral da autenticação Kerberos](kerberos-authentication-overview.md)
