---
title: "Autenticação de chave pública do dispositivo do domínio"
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 8/18/2017
ms.openlocfilehash: 65f39f4900fcd99ef90b160d3db7099edb73bc1c
ms.sourcegitcommit: e94838df702ff0135ebe7c179b228aa67b772d35
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2017
---
# <a name="domain-joined-device-public-key-authentication"></a>Autenticação de chave pública do dispositivo do domínio

>Aplica-se a: Windows Server 2016, Windows 10

Kerberos adicionado o suporte para dispositivos que ingressaram no domínio entrar na rede usando um início de certificado com o Windows Server 2012 e Windows 8. Essa alteração permite que fornecedores 3ª criar soluções para provisionar e inicialize certificados para dispositivos que ingressaram no domínio a ser usado para autenticação de domínio. 

## <a name="automatic-public-key-provisioning"></a>Pública chave provisionamento automático

A partir do Windows 10 versão 1507 e Windows Server 2016, dispositivos que ingressaram no domínio provisionem automaticamente uma chave pública associada a um controlador de domínio do Windows Server 2016 (DC). Depois que uma chave é provisionada, Windows pode usar autenticação de chave pública para o domínio.

### <a name="public-key-generation"></a>Geração de chaves públicas
Se o dispositivo está executando o Credential Guard, uma chave pública é criada protegida pelo Credential Guard. 

Se o Credential Guard não está disponível e um TPM, uma chave pública é criada protegida pelo TPM. 

Se nenhuma estiver disponível, uma chave não é gerada e o dispositivo só pode autenticar usando a senha.

### <a name="provisioning-computer-account-public-key"></a>Provisionamento de chave pública de conta de computador
Quando o Windows é iniciado, ele verifica se uma chave pública é provisionada para sua conta de computador. Caso contrário, em seguida, ele gera uma chave pública associada e configura para sua conta no AD usando um Windows Server 2016 ou superior DC. Se todos os controladores de domínio para baixo nível, nenhuma chave é provisionada.

### <a name="configuring-device-to-only-use-public-key"></a>Configurando o dispositivo para usar apenas a chave pública
Se a configuração de política de grupo **suporte para autenticação de dispositivo usando o certificado** é definido como **força**, em seguida, o dispositivo precisa encontrar um controlador de domínio que executa o Windows Server 2016 ou posterior para autenticar. A configuração estiver sob modelos administrativos > sistema > Kerberos.

### <a name="configuring-device-to-only-use-password"></a>Configurando o dispositivo para usar somente a senha
Se a configuração de política de grupo **suporte para autenticação de dispositivo usando o certificado** está desabilitada, senha sempre é usada. A configuração estiver sob modelos administrativos > sistema > Kerberos.

## <a name="domain-joined-device-authentication-using-public-key"></a>Autenticação de dispositivo ingressado no domínio usando a chave pública
Quando o Windows tem um certificado para o dispositivo ingressado no domínio, Kerberos autentica pela primeira vez usando o certificado e em repetições falha com senha. Isso permite que o dispositivo para se autenticar em controladores de domínio de nível inferior.

Como as chaves públicas automaticamente provisionadas têm um certificado autoassinado, validação do certificado falhará em controladores de domínio que não dão suporte a mapeamento de conta de confiar em chave. Por padrão, o Windows tentará novamente a autenticação usando a senha de domínio do dispositivo.


