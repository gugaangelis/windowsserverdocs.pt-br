---
title: Configurar modelos de certificado para requisitos de PEAP e EAP
description: Este tópico fornece informações sobre como usar certificados com o servidor de políticas de rede e acesso remoto no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 41f6f88705fa3d58be695fd825d960e7df21cd24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885877"
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>Configurar modelos de certificado para requisitos de PEAP e EAP

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Todos os certificados que são usados para autenticação de acesso de rede com o protocolo de autenticação extensível\-Transport Layer Security \(EAP\-TLS\), Protected Extensible Authentication Protocol\- Transport Layer Security \(PEAP\-TLS\)e o PEAP\-Microsoft Challenge Handshake Authentication Protocol versão 2 \(MS\-CHAP v2\) deve atender aos requisitos de certificados x. 509 e trabalhar para conexões que usam Secure Socket Layer/Transport nível de segurança (SSL/TLS). Certificados de cliente e servidor têm requisitos adicionais.

>[!IMPORTANT]
>Este tópico fornece instruções sobre como configurar modelos de certificado. Para usar essas instruções, é necessário que você implantou sua própria infra-estrutura de chave pública \(PKI\) com o Active Directory Certificate Services \(AD CS\).

## <a name="minimum-server-certificate-requirements"></a>Requisitos de certificado de servidor mínima

Com o PEAP\-MS\-CHAP v2, PEAP\-TLS ou EAP\-TLS como método de autenticação, o NPS deve usar um certificado de servidor que atenda aos requisitos de certificado do servidor mínima. 

Computadores cliente podem ser configurados para validar certificados de servidor usando o **Validar certificado do servidor** opção no computador cliente ou na diretiva de grupo. 

O computador cliente aceita a tentativa de autenticação do servidor quando o certificado do servidor atende aos seguintes requisitos:

- O nome da entidade contém um valor. Se você emitir um certificado para o servidor que executa o servidor de diretivas de rede (NPS) que tem um nome de assunto em branco, o certificado não está disponível para autenticar o NPS. Para configurar o modelo de certificado com um nome de assunto:

    1. Abra os Modelos de Certificado.
    2. No painel de detalhes, clique com botão direito o modelo de certificado que você deseja alterar e, em seguida, clique em **propriedades** .
    3. Clique o **nome da entidade** guia e, em seguida, clique em **compilar com as informações do Active Directory do**.
    4. Na **formato de nome de assunto**, selecione um valor diferente de **None**.

- O certificado de computador do servidor está vinculado a uma autoridade de certificação (CA) raiz confiável e não apresenta falhas nas verificações executadas por CryptoAPI e que são especificadas na política de acesso remoto ou diretiva de rede.

- O certificado de computador para o servidor NPS ou VPN está configurado com a finalidade de autenticação de servidor nas extensões de uso estendido de chave (EKU). (O identificador de objeto para autenticação de servidor é 1.3.6.1.5.5.7.3.1.)

- Configure o certificado do servidor com a configuração de criptografia necessária:

    1. Abra os Modelos de Certificado.
    2. No painel de detalhes, clique com botão direito o modelo de certificado que você deseja alterar e, em seguida, clique em **propriedades**.
    3. Clique o **Cryptography** guia e certifique-se configurar o seguinte:
       - **Categoria de provedor:** Provedor de Armazenamento de Chaves
       - **Nome do algoritmo:** RSA
       - **Provedores:** Provedor de criptografia da plataforma Microsoft
       - **Tamanho mínimo da chave:** 2048
       - **Algoritmo de hash:** SHA2
    4. Clique em **Avançar**.

- A extensão de nome alternativo da entidade (SubjectAltName), se usado, deve conter o nome DNS do servidor. Para configurar o modelo de certificado com o nome do sistema de nome de domínio (DNS) do servidor de registro: 

    1. Abra os Modelos de Certificado.
    2. No painel de detalhes, clique com botão direito o modelo de certificado que você deseja alterar e, em seguida, clique em **propriedades** .
    3. Clique o **nome da entidade** guia e, em seguida, clique em **compilar com as informações do Active Directory do**.
    4. Na **incluir esta informação no nome alternativo da entidade**, selecione **nome DNS**.

Ao usar o protocolo PEAP e EAP-TLS, NPSs exibir uma lista de todos os certificados instalados no repositório de certificados de computador, com as seguintes exceções:

- Certificados que não contêm a finalidade de autenticação de servidor em extensões EKU não são exibidos.

- Certificados que não contêm um nome de entidade não são exibidos.

- Com base no registro e certificados de logon de cartão inteligente não são exibidos.

Para obter mais informações, consulte [implantar certificados de servidor para 802.1 X com fio e implantações sem fio](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="minimum-client-certificate-requirements"></a>Requisitos de certificado mínima do cliente

Com o EAP-TLS ou PEAP-TLS, o servidor aceita a tentativa de autenticação de cliente quando o certificado atender aos seguintes requisitos:

- O certificado do cliente é emitido por uma autoridade de certificação corporativa ou mapeado para uma conta de usuário ou computador nos serviços de domínio do Active Directory \(AD DS\).

- O certificado de usuário ou computador em que o cliente se encadeia a uma autoridade de certificação de raiz confiável inclui a finalidade de autenticação de cliente em extensões EKU \(o identificador de objeto para autenticação de cliente é 1.3.6.1.5.5.7.3.2\)e falha nem o verificações que são executadas por CryptoAPI e que são especificadas na diretiva de acesso remoto ou diretiva de rede nem as verificações de identificador de objeto de certificado especificadas na diretiva de rede do NPS.

- O cliente 802.1 X não usa certificados baseados no registro que são o logon de cartão inteligente ou certificados protegidos por senha.

- Para certificados de usuário, o nome alternativo da entidade \(SubjectAltName\) extensão no certificado contém o nome UPN \(UPN\). Para configurar o UPN em um modelo de certificado:

    1. Abra os Modelos de Certificado.
    2. No painel de detalhes, clique com botão direito o modelo de certificado que você deseja alterar e, em seguida, clique em **propriedades**.
    3. Clique o **nome da entidade** guia e, em seguida, clique em **compilar com as informações do Active Directory do**.
    4. Na **incluir esta informação no nome alternativo da entidade**, selecione **nome UPN \(UPN\)**.

- Para certificados de computador, o nome alternativo da entidade \(SubjectAltName\) extensão no certificado deve conter o nome de domínio totalmente qualificado \(FQDN\) do cliente, que também é chamado de  *Nome DNS*. Para configurar esse nome no modelo de certificado:

    1. Abra os Modelos de Certificado.
    2. No painel de detalhes, clique com botão direito o modelo de certificado que você deseja alterar e, em seguida, clique em **propriedades**.
    3. Clique o **nome da entidade** guia e, em seguida, clique em **compilar com as informações do Active Directory do**.
    4. Na **incluir esta informação no nome alternativo da entidade**, selecione **nome DNS**.

Com o PEAP\-TLS e EAP\-TLS, os clientes exibem uma lista de todos os certificados instalados no snap-in de certificados, com as seguintes exceções:

- Os clientes sem fio não são exibidos com base no registro e certificados de logon de cartão inteligente. 

- Os clientes sem fio e clientes VPN não exibem certificados protegidos por senha. 

- Certificados que não contêm a finalidade de autenticação de cliente em extensões EKU não são exibidos.


Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
