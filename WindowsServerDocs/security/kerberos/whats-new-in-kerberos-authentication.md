---
title: O que há de novo na autenticação Kerberos
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: justinha
ms.technology: security-authentication
ms.date: 11/09/2016
ms.openlocfilehash: 90107bd49268f232fd6d532c304c2fdd050bcbf5
ms.sourcegitcommit: c6acac3622e5d34714ca5c569805931681f98779
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391502"
---
# <a name="whats-new-in-kerberos-authentication"></a>What's New in Kerberos Authentication

>Aplica-se a: Windows Server 2016 e Windows 10

## <a name="kdc-support-for-public-key-trust-based-client-authentication"></a>Suporte para autenticação de cliente com base na chave pública de confiança do KDC

Começando com o Windows Server 2016, os KDCs dão suporte a uma forma de mapeamento de chave pública. Se a chave pública é provisionada para uma conta, o KDC dá suporte a Kerberos PKInit explicitamente usando essa chave. Como não há nenhuma validação de certificado, os certificados autoassinados têm suporte e não há suporte para a garantia do mecanismo de autenticação.

Chave de confiança é preferível quando configurado para uma conta, independentemente da configuração UseSubjectAltName.

## <a name="kerberos-client-and-kdc-support-for-rfc-8070-pkinit-freshness-extension"></a>Cliente Kerberos e o KDC oferecem suporte para RFC 8070 PKInit atualização de extensão

Começando com o Windows 10, versão 1607 e Windows Server 2016, a tentativa de clientes Kerberos com o [extensão de atualização de RFC 8070 PKInit](https://datatracker.ietf.org/doc/draft-ietf-kitten-pkinit-freshness/) para chave pública baseados em logons. 

Começando com o Windows Server 2016, os KDCs podem dar suporte a extensão de atualização de PKInit. Por padrão, os KDCs não oferecem a extensão de atualização de PKInit. Para habilitá-lo, use o novo suporte KDC para configuração de diretiva de modelo administrativo KDC de extensão de atualização de PKInit em todos os DCs no domínio. Quando configurado, as opções a seguir têm suporte quando o domínio é o nível funcional de domínio do Windows Server 2016 (DFL):

- **Desabilitado**: O KDC nunca oferece a extensão de atualização de PKInit e aceita as solicitações de autenticação válido sem verificação para atualização. Os usuários nunca receberá a identidade de chave pública nova SID.
- **Suporte para**: Extensão de atualização de PKInit é compatível com a solicitação. Os clientes do Kerberos autenticar com êxito com a extensão de atualização de PKInit recebem a identidade de chave pública nova SID.
- **Necessário**: Extensão de atualização de PKInit é necessária para a autenticação bem-sucedida. Os clientes do Kerberos que não dão suporte a extensão de atualização de PKInit sempre irá falhar ao usar as credenciais de chave públicas.

## <a name="domain-joined-device-support-for-authentication-using-public-key"></a>Suporte a dispositivos ingressados no domínio para autenticação usando a chave pública

Começando com o Windows 10 versão 1507 e Windows Server 2016, se um dispositivo ingressado no domínio é capaz de registrar sua chave pública associado um controlador de domínio (DC) do Windows Server 2016, em seguida, o dispositivo pode autenticar com a chave pública usando a autenticação Kerberos um controlador de domínio Windows Server 2016. Para obter mais informações, consulte [ingressado no domínio público chave de autenticação de dispositivo](Domain-joined-Device-Public-Key-Authentication.md)

## <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Os clientes do Kerberos permitem nomes de host de endereço de IPv4 e IPv6 em nomes de entidade de serviço (SPNs)

Começando com o Windows 10 versão 1507 e Windows Server 2016, clientes Kerberos podem ser configurados para dar suporte a nomes de host de IPv4 e IPv6 no SPNs. 

Caminho do registro:

HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters

Para configurar o suporte para nomes de host de endereço IP no SPNs, crie uma entrada de TryIPSPN. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. Se não configurado, os nomes de host de endereço IP não serão tentadas.

Se o SPN é registrado no Active Directory, autenticação foi bem-sucedida com o Kerberos. 

Para obter mais informações Confira o documento [Configurando o Kerberos para endereços IP](configuring-kerberos-over-ip.md).

## <a name="kdc-support-for-key-trust-account-mapping"></a>Suporte KDC para mapeamento de conta de confiança de chave

Começando com o Windows Server 2016, os controladores de domínio têm suporte para mapeamento de conta de confiança de chave, bem como fallback para o nome Principal do usuário (UPN) e AltSecID existentes no comportamento de SAN. Quando UseSubjectAltName é definida como:

- 0: Mapeamento explícito é necessário. Em seguida, deve haver um:
    - Chave de confiança (novo no Windows Server 2016)
    - ExplicitAltSecID
- 1: Mapeamento implícito é permitido (padrão):
    1. Se confiar em chave estiver configurado para a conta, em seguida, ele é usado para mapear (novo com o Windows Server 2016).
    2. Se não houver nenhum UPN no SAN, AltSecID é tentada para mapeamento.
    3. Se houver um UPN na SAN, o UPN é tentada para mapeamento.

## <a name="see-also"></a>Consulte também

- [Visão geral da autenticação do Kerberos](kerberos-authentication-overview.md)
