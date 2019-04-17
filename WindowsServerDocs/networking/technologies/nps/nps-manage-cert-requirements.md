---
title: Configurar modelos de certificado para PEAP e EAP requisitos
description: Este tópico fornece informações sobre como usar certificados com o servidor de políticas de rede e acesso remoto no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e597a65982aeead907c9a41f17f1785a0bf81016
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>Configurar modelos de certificado para PEAP e EAP requisitos

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Todos os certificados que são usados para autenticação de acesso de rede com \(EAP\-TLS\) Extensible Authentication Protocol\-Transport Layer Security, protegido Extensible Authentication Protocol\ Transport Layer Security \(PEAP\-TLS\) e PEAP\ Microsoft Challenge Handshake Authentication Protocol versão 2 \ (CHAP MS\ v2\) deve atender aos requisitos de certificados x. 509 e trabalhar para conexões que usam Secure Socket Layer/Transport nível Security (TLS/SSL). Certificados de cliente e servidor têm requisitos adicionais.

>[!IMPORTANT]
>Este tópico fornece instruções para configurar os modelos de certificado. Para usar essas instruções, é necessário que você implantou \(PKI\) sua própria infraestrutura de chave pública com serviços de certificados do Active Directory \(AD CS\).

## <a name="minimum-server-certificate-requirements"></a>Requisitos de certificado do servidor mínimo

Com PEAP\-MS\-CHAP v2, TLS PEAP\ ou EAP\-TLS como o método de autenticação, o servidor NPS deve usar um certificado de servidor que atenda aos requisitos de certificado do servidor mínimo. 

Computadores cliente podem ser configurados para validar certificados de servidor usando o **Validar certificado do servidor** opção no computador cliente ou na política de grupo. 

O computador cliente aceita a tentativa de autenticação do servidor quando o certificado do servidor atenda aos seguintes requisitos:

- O nome do requerente contém um valor. Se você emitir um certificado de servidor que executa o servidor de política de rede (NPS) que tem um nome de assunto em branco, o certificado não está disponível para autenticar o servidor NPS. Para configurar o modelo de certificado com um nome de entidade:

    1. Abra modelos de certificado.
    2. No painel de detalhes, clique com botão direito do modelo de certificado que você deseja alterar e, em seguida, clique em **propriedades** .
    3. Clique no **nome do requerente** guia e, em seguida, clique em **compilar a partir dessas informações do Active Directory**.
    4. Em **formato do nome do assunto**, selecione um valor diferente de **None**.

- O certificado de computador no servidor está vinculado a uma autoridade de certificação (CA) raiz confiável e não apresenta falhas nas verificações que são executadas por CryptoAPI e que são especificadas na política de acesso remoto ou política de rede.

- O certificado de computador para o servidor NPS ou o servidor VPN é configurado com a finalidade de autenticação de servidor em extensões de uso de chave estendido (EKU). (O identificador de objeto para autenticação de servidor é 1.3.6.1.5.5.7.3.1.)

- O certificado do servidor está configurado com um valor de algoritmo obrigatório de **RSA**. Para definir a configuração de criptografia necessárias:

    1. Abra modelos de certificado.
    2. No painel de detalhes, clique com botão direito do modelo de certificado que você deseja alterar e, em seguida, clique em **propriedades**.
    3. Clique no **criptografia** guia. Em **nome do algoritmo**, clique em **RSA**. Certifique-se de que **tamanho mínimo da chave** é definido como **2048**.

- A extensão de nome alternativo (SubjectAltName), se usado, deve conter o nome DNS do servidor. Para configurar o modelo de certificado com o nome do sistema de nome de domínio (DNS) do servidor de registro: 

    1. Abra modelos de certificado.
    2. No painel de detalhes, clique com botão direito do modelo de certificado que você deseja alterar e, em seguida, clique em **propriedades** .
    3. Clique no **nome do requerente** guia e, em seguida, clique em **compilar a partir dessas informações do Active Directory**.
    4. Em **incluir esta informação no nome do requerente alternativo**, selecione **nome DNS**.

Ao usar PEAP e EAP-TLS, os servidores NPS exibem uma lista de todos os certificados instalados no repositório de certificados de computador, com as seguintes exceções:

- Certificados que não contêm a finalidade de autenticação de servidor em extensões EKU não são exibidos.

- Certificados que não contêm um nome de entidade não são exibidos.

- Baseadas no registro e certificados de cartão inteligente logon não são exibidos.

Para obter mais informações, consulte [implantar certificados de servidor para 802.1 X com e sem fio implantações](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="minimum-client-certificate-requirements"></a>Requisitos de certificado do cliente mínimo

Com EAP-TLS ou PEAP-TLS, o servidor aceita a tentativa de autenticação do cliente quando o certificado atenda aos seguintes requisitos:

- O certificado do cliente é emitido por uma autoridade de certificação corporativa ou mapeado para uma conta de usuário ou computador no Active Directory Domain Services \(AD DS\).

- O certificado de usuário ou computador no cliente encadeia com uma raiz confiável CA, inclui a finalidade de autenticação de cliente em extensões EKU \ (o identificador de objeto para autenticação de cliente é 1.3.6.1.5.5.7.3.2\) e falha as verificações que são executadas por CryptoAPI e que são especificados na política de acesso remoto ou política de rede nem as verificações de identificador de objeto de certificado que são especificadas na política de rede NPS.

- O cliente 802.1 X não usa certificados baseadas no registro que são logon de cartão inteligente ou certificados protegidos por senha.

- Certificados de usuário, a extensão de \(SubjectAltName\) nome alternativo no certificado contém a entidade de segurança do usuário nomear \(UPN\). Para configurar o UPN em um modelo de certificado:

    1. Abra modelos de certificado.
    2. No painel de detalhes, clique com botão direito do modelo de certificado que você deseja alterar e, em seguida, clique em **propriedades**.
    3. Clique no **nome do requerente** guia e, em seguida, clique em **compilar a partir dessas informações do Active Directory**.
    4. Em **incluir esta informação no nome do requerente alternativo**, selecione **UPN nomear \(UPN\)**.

- Certificados de computador, a extensão de \(SubjectAltName\) nome alternativo no certificado deve conter o domínio totalmente qualificado \(FQDN\) do cliente, que também é chamado de nomear o *nome DNS*. Para configurar esse nome no modelo de certificado:

    1. Abra modelos de certificado.
    2. No painel de detalhes, clique com botão direito do modelo de certificado que você deseja alterar e, em seguida, clique em **propriedades**.
    3. Clique no **nome do requerente** guia e, em seguida, clique em **compilar a partir dessas informações do Active Directory**.
    4. Em **incluir esta informação no nome do requerente alternativo**, selecione **nome DNS**.

Com PEAP\-TLS e EAP\ TLS, os clientes exibem uma lista de todos os certificados instalados no snap-in de certificados, com as seguintes exceções:

- Os clientes sem fio não exibir baseadas no registro e certificados de logon de cartão inteligente. 

- Clientes sem fio e clientes VPN não exibem certificados protegidos por senha. 

- Certificados que não contêm a finalidade de autenticação de cliente em extensões EKU não são exibidos.


Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).
