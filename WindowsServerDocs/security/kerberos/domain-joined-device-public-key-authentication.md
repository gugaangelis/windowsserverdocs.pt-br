---
title: Autenticação de chave pública de dispositivo ingressado no domínio
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 7bd17803-6e42-4a3b-803f-e47c74725813
manager: alanth
author: michikos
ms.technology: security-authentication
ms.date: 08/18/2017
ms.openlocfilehash: 80906e7cfe3740200938704a4b4eaf0759af303a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885027"
---
# <a name="domain-joined-device-public-key-authentication"></a>Autenticação de chave pública de dispositivo ingressado no domínio

>Aplica-se a: Windows Server 2016, Windows 10

Kerberos adicionou suporte para dispositivos ingressados em domínio para entrar usando um certificado que começa com o Windows Server 2012 e Windows 8. Essa alteração permite que fornecedores 3ª criar soluções para provisionar e inicializar certificados para dispositivos ingressados no domínio a ser usado para autenticação de domínio. 

## <a name="automatic-public-key-provisioning"></a>Público chave o provisionamento automático

Começando com o Windows 10 versão 1507 e Windows Server 2016, dispositivos de domínio provisiona automaticamente uma chave pública associada a um controlador de domínio do Windows Server 2016 (DC). Depois que uma chave é fornecida, em seguida, o Windows podem usar a autenticação de chave pública para o domínio.

### <a name="public-key-generation"></a>Geração de chave pública
Se o dispositivo está executando o Credential Guard, é uma chave pública criada protegida pelo Credential Guard. 

Se o Credential Guard não está disponível e é de um TPM, uma chave pública é criada protegida pelo TPM. 

Se nenhuma estiver disponível, em seguida, uma chave não é gerada e o dispositivo só pode autenticar usando a senha.

### <a name="provisioning-computer-account-public-key"></a>Provisionamento de chave pública de conta de computador
Quando o Windows for iniciado, ele verifica se uma chave pública é provisionada para sua conta de computador. Caso contrário, em seguida, gera uma chave pública associada e o configura para sua conta no AD usando um Windows Server 2016 ou o controlador de domínio superior. Se todos os controladores de domínio de nível inferior, nenhuma chave é provisionada.

### <a name="configuring-device-to-only-use-public-key"></a>Configuração de dispositivo para usar somente a chave pública
Se a configuração de diretiva de grupo **suporte para autenticação de dispositivo usando o certificado** é definido como **força**, em seguida, o dispositivo precisa encontrar um controlador de domínio que executa o Windows Server 2016 ou posterior para se autenticar. A configuração é em modelos administrativos > sistema > Kerberos.

### <a name="configuring-device-to-only-use-password"></a>Configuração de dispositivo para usar apenas a senha
Se a configuração de diretiva de grupo **suporte para autenticação de dispositivo usando o certificado** estiver desabilitado, em seguida, a senha é sempre usada. A configuração é em modelos administrativos > sistema > Kerberos.

## <a name="domain-joined-device-authentication-using-public-key"></a>Autenticação de dispositivo ingressado no domínio usando a chave pública
Quando o Windows tem um certificado para o dispositivo ingressado no domínio, Kerberos primeiro autentica usando o certificado e em novas tentativas de falha com senha. Isso permite que o dispositivo se autentique para controladores de domínio de nível inferior.

Como as chaves públicas automaticamente provisionadas tem um certificado autoassinado, validação de certificado falhará em controladores de domínio que não dão suporte a mapeamento de conta de confiança de chave. Por padrão, o Windows tentará novamente a autenticação usando a senha do domínio do dispositivo.


