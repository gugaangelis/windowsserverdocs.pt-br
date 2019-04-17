---
title: "Novidades na autenticação Kerberos"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 7b046490c606cdf9e1436f503bf46a9cd4280ea9
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/13/2018
---
# <a name="whats-new-in-kerberos-authentication"></a>Novidades na autenticação Kerberos

>Aplica-se a: Windows Server 2016 e Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>Suporte do KDC confiar pública da chave de autenticação de cliente

A partir do Windows Server 2016, KDCs dão suporte a uma forma de mapeamento de chave pública. Se a chave pública é provisionada para uma conta, KDC dá suporte a Kerberos PKInit explicitamente usando essa chave. Como não há nenhuma validação de certificado, há suporte para certificados autoassinados e garantia do mecanismo de autenticação não é suportada.

Chave confiável é preferencial quando configurado para uma conta, independentemente da configuração UseSubjectAltName.

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Suportam de cliente Kerberos e KDC RFC 8070 PKInit renovação de extensão

A partir do Windows 10, versão 1607 e Windows Server 2016, os clientes de Kerberos tentam o [extensão de renovação do RFC 8070 PKInit](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/) de chave pública com base em logons. 

A partir do Windows Server 2016, KDCs pode aceitar a extensão de renovação PKInit. Por padrão, KDCs não oferecem a extensão de renovação PKInit. Para habilitá-lo, use o novo suporte KDC para configuração de política de modelo administrativo PKInit renovação extensão KDC em todos os controladores de domínio no domínio. Quando configurada, as opções a seguir têm suporte quando o domínio é o nível funcional do domínio do Windows Server 2016 (DFL):

- **Desabilitado**: O KDC nunca oferece a extensão de renovação de PKInit e aceita as solicitações de autenticação válido sem verificando o grau de atualização. Os usuários nunca receberá a identidade de chave pública nova SID.
- **Suporte**: PKInit renovação de extensão é compatível com a solicitação. Os clientes de Kerberos autentiquem com êxito com a extensão de renovação de PKInit receber a identidade de chave pública nova SID.
- **Obrigatório**: PKInit renovação de extensão é necessária para autenticação bem-sucedida. Os clientes de Kerberos não compatíveis com a extensão de renovação de PKInit sempre falhará ao usar credenciais de chave públicas.

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Suporte a dispositivos de domínio para autenticação usando a chave pública

Começando com o Windows 10 versão 1507 e Windows Server 2016, se um dispositivo ingressado no domínio é capaz de registrar sua chave pública associada com um controlador de domínio do Windows Server 2016 (DC), em seguida, o dispositivo pode autenticar com a chave pública usando a autenticação Kerberos para um controlador de domínio do Windows Server 2016. Para obter mais informações, consulte [domínio pública chave de autenticação de dispositivo](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Os clientes Kerberos permitem hostnames endereço IPv4 e IPv6 nos nomes de entidade de serviço (SPNs)

A partir do Windows 10 versão 1507 e Windows Server 2016, os clientes Kerberos podem ser configurados para dar suporte a nomes de host IPv4 e IPv6 SPNs. 

Caminho do registro:

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

Para configurar o suporte para nomes de host de endereço IP em SPNs, crie uma entrada TryIPSPN. Essa entrada não existir no registro por padrão. Depois que tiver criado a entrada, altere o valor DWORD como 1. Se não estiver configurado, nomes de host de endereço IP não são aplicados.

Se o SPN é registrado no Active Directory, autenticação é bem-sucedida com Kerberos. 

## <a name="kdc-support-for-key-trust-account-mapping"></a>Suporte KDC para confiar em chave mapeamento de conta

A partir do Windows Server 2016, controladores de domínio têm suporte para mapeamento de conta de confiar em chave, bem como fallback para o nome Principal do usuário (UPN) e AltSecID existentes no comportamento SAN. Quando UseSubjectAltName é definido como:

- 0: mapeamento explícito é necessário. Em seguida, deve haver um:
    - Chave confiável (novo no Windows Server 2016)
    - ExplicitAltSecID
- 1: mapeamento implícito é permitido (padrão):
    1. Se confiar chave estiver configurado para conta, em seguida, ele é usado para ter um mapeamento (Novidades do Windows Server 2016).
    2. Se não houver nenhum UPN na SAN, AltSecID é tentada para ter um mapeamento.
    3. Se existir um UPN na SAN, UPN é tentada para ter um mapeamento.

## <a name="see-also"></a>Consulte também

- [Visão geral de autenticação Kerberos](kerberos-authentication-overview.md)
