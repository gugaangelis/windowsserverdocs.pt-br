---
title: Configurar modelos de certificado para requisitos de PEAP e EAP
description: Este tópico fornece informações sobre como usar certificados com o servidor de políticas de rede e acesso remoto no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2af0a1df-5c44-496b-ab11-5bc340dc96f0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f60e5a1da1388a6dd2432a3603f83d6ca2990a29
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405402"
---
# <a name="configure-certificate-templates-for-peap-and-eap-requirements"></a>Configurar modelos de certificado para requisitos de PEAP e EAP

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Todos os certificados que são usados para autenticação de acesso à rede com o protocolo de autenticação extensível @ no__t-0Transport Layer Security \(EAP @ no__t-2TLS @ no__t-3, protocolo de autenticação extensível protegido @ no__t-4Transport Layer Security \(PEAP @ no__t-6TLS @ no__t-7 e PEAP @ no__t-8Microsoft Challenge Handshake Authentication Protocol versão 2 \(MS @ no__t-10CHAP v2 @ no__t-11 deve atender aos requisitos para certificados X. 509 e trabalhar para conexões que usam soquete seguro Segurança de nível de transporte/camada (SSL/TLS). Os certificados de cliente e de servidor têm requisitos adicionais.

>[!IMPORTANT]
>Este tópico fornece instruções para configurar modelos de certificado. Para usar essas instruções, é necessário que você tenha implantado sua própria infraestrutura de chave pública \(PKI @ no__t-1 com Active Directory serviços de certificados \(AD CS @ no__t-3.

## <a name="minimum-server-certificate-requirements"></a>Requisitos mínimos de certificado do servidor

Com o PEAP @ no__t-0MS @ no__t-1CHAP v2, o PEAP @ no__t-2TLS ou o EAP @ no__t-3TLS como o método de autenticação, o NPS deve usar um certificado de servidor que atenda aos requisitos mínimos de certificado do servidor. 

Os computadores cliente podem ser configurados para validar certificados de servidor usando a opção **validar certificado do servidor** no computador cliente ou no política de grupo. 

O computador cliente aceita a tentativa de autenticação do servidor quando o certificado do servidor atende aos seguintes requisitos:

- O nome da entidade contém um valor. Se você emitir um certificado para o servidor que executa o NPS (servidor de diretivas de rede) que tem um nome de entidade em branco, o certificado não estará disponível para autenticar o NPS. Para configurar o modelo de certificado com um nome de entidade:

    1. Abra os Modelos de Certificado.
    2. No painel de detalhes, clique com o botão direito do mouse no modelo de certificado que você deseja alterar e clique em **Propriedades** .
    3. Clique na guia **nome da entidade** e, em seguida, clique em **criar com base nessa Active Directory informações**.
    4. Em **formato de nome da entidade**, selecione um valor diferente de **nenhum**.

- O certificado do computador no servidor se encadeia a uma AC (autoridade de certificação) raiz confiável e não falha em nenhuma das verificações executadas pelo CryptoAPI e que são especificadas na política de acesso remoto ou na diretiva de rede.

- O certificado de computador para o servidor NPS ou VPN é configurado com a finalidade de autenticação de servidor em extensões EKU (uso estendido de chave). (O identificador de objeto para autenticação de servidor é 1.3.6.1.5.5.7.3.1.)

- Configure o certificado do servidor com a configuração de criptografia necessária:

    1. Abra os Modelos de Certificado.
    2. No painel de detalhes, clique com o botão direito do mouse no modelo de certificado que você deseja alterar e clique em **Propriedades**.
    3. Clique na guia **criptografia** e certifique-se de configurar o seguinte:
       - **Categoria do provedor:** Provedor de Armazenamento de Chaves
       - **Nome do algoritmo:** RSA
       - **Fornecedor** Provedor Microsoft Platform crypto
       - **Tamanho mínimo da chave:** 2048
       - **Algoritmo de hash:** SHA2
    4. Clique em **Avançar**.

- A extensão de nome alternativo da entidade (SubjectAltName), se usada, deve conter o nome DNS do servidor. Para configurar o modelo de certificado com o nome DNS (sistema de nomes de domínio) do servidor de registro: 

    1. Abra os Modelos de Certificado.
    2. No painel de detalhes, clique com o botão direito do mouse no modelo de certificado que você deseja alterar e clique em **Propriedades** .
    3. Clique na guia **nome da entidade** e, em seguida, clique em **criar com base nessa Active Directory informações**.
    4. Em **incluir essas informações em nome de entidade alternativo**, selecione **nome DNS**.

Ao usar PEAP e EAP-TLS, o NPSs exibirá uma lista de todos os certificados instalados no repositório de certificados do computador, com as seguintes exceções:

- Os certificados que não contêm a finalidade de autenticação de servidor em extensões EKU não são exibidos.

- Os certificados que não contêm um nome de entidade não são exibidos.

- Os certificados de logon e de cartão inteligente com base no registro não são exibidos.

Para obter mais informações, consulte [implantar certificados de servidor para implantações com e sem fio 802.1 x](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments).

## <a name="minimum-client-certificate-requirements"></a>Requisitos mínimos de certificado do cliente

Com EAP-TLS ou PEAP-TLS, o servidor aceita a tentativa de autenticação de cliente quando o certificado atende aos seguintes requisitos:

- O certificado do cliente é emitido por uma autoridade de certificação corporativa ou mapeado para uma conta de usuário ou computador no Active Directory Domain Services \(AD DS @ no__t-1.

- O certificado de usuário ou computador no cliente encadeia-se a uma AC raiz confiável, inclui a finalidade de autenticação de cliente em extensões EKU @no__t o identificador de objeto 0the para autenticação de cliente é 1.3.6.1.5.5.7.3.2 @ no__t-1 e não apresenta as verificações que estão executado pelo CryptoAPI e que são especificados na política de acesso remoto ou diretiva de rede, nem nas verificações de identificador de objeto de certificado especificadas na política de rede do NPS.

- O cliente 802.1 X não usa certificados baseados no registro que sejam certificados de logon de cartão inteligente ou protegidos por senha.

- Para certificados de usuário, o nome alternativo da entidade \(SubjectAltName @ no__t-1 extensão no certificado contém o nome principal do usuário \(UPN @ no__t-3. Para configurar o UPN em um modelo de certificado:

    1. Abra os Modelos de Certificado.
    2. No painel de detalhes, clique com o botão direito do mouse no modelo de certificado que você deseja alterar e clique em **Propriedades**.
    3. Clique na guia **nome da entidade** e, em seguida, clique em **criar com base nessa Active Directory informações**.
    4. Em **incluir essas informações em nome de entidade alternativo**, selecione **nome principal de usuário \(UPN @ no__t-3**.

- Para certificados de computador, o nome alternativo da entidade \(SubjectAltName @ no__t-1 extensão no certificado deve conter o nome de domínio totalmente qualificado \(FQDN @ no__t-3 do cliente, que também é chamado de *nome DNS*. Para configurar esse nome no modelo de certificado:

    1. Abra os Modelos de Certificado.
    2. No painel de detalhes, clique com o botão direito do mouse no modelo de certificado que você deseja alterar e clique em **Propriedades**.
    3. Clique na guia **nome da entidade** e, em seguida, clique em **criar com base nessa Active Directory informações**.
    4. Em **incluir essas informações em nome de entidade alternativo**, selecione **nome DNS**.

Com o PEAP @ no__t-0TLS e o EAP @ no__t-1TLS, os clientes exibem uma lista de todos os certificados instalados no snap-in de certificados, com as seguintes exceções:

- Os clientes sem fio não exibem certificados baseados no registro e de logon de cartão inteligente. 

- Clientes sem fio e clientes VPN não exibem certificados protegidos por senha. 

- Os certificados que não contêm a finalidade de autenticação de cliente em extensões EKU não são exibidos.


Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
