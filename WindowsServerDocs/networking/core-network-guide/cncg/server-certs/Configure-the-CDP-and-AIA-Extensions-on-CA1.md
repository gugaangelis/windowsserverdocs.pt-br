---
title: Configurar o CDP e AIA extensões em CA1
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 80f6d68816a58b2ac30010e0917ce0f816a30234
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>Configurar o CDP e AIA extensões em CA1

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para configurar o ponto de distribuição de lista de revogação de certificados (CRL) (CDP) e as configurações de acesso de informações da autoridade (AIA) em CA1.  
  
Para executar este procedimento, você deve ser um membro do grupo Administradores de domínio.  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>Para configurar as extensões CDP e AIA em CA1  
  
1.  No Gerenciador do servidor, clique em **ferramentas** e, em seguida, clique em **autoridade de certificação**.  
  
2.  Na árvore de console autoridade de certificação, clique com botão direito **corp-CA1-CA**e clique em **propriedades**.  
  
    > [!NOTE]  
    > O nome da autoridade de certificação é diferente se você não nomear o computador CA1 e seu nome de domínio é diferente neste exemplo. O nome da CA está no formato *domínio*-*Nomecomputadorca*-CA.  
  
3.  Clique no **extensões** guia Certifique-se de que **selecione extensão** é definido como **ponto de distribuição da lista de certificados Revogados**e no **especificar locais onde os usuários podem obter uma lista de certificados revogados (CRL)**, faça o seguinte:  
  
    1.  Selecione a entrada `file:\/\/\\\\<ServerDNSName>\/CertEnroll\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`e clique em **remover**. Em **Confirmar remoção**, clique em **Sim**.  
  
    2.  Selecione a entrada `http:\/\/<ServerDNSName>\/CertEnroll\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`e clique em **remover**. Em **Confirmar remoção**, clique em **Sim**.  
  
    3.  Selecione a entrada que começa com o caminho `ldap:\/\/CN\=<CATruncatedName><CRLNameSuffix>,CN\=<ServerShortName>`e clique em **remover**. Em **Confirmar remoção**, clique em **Sim**.  
  
4.  Em **especificar locais onde os usuários podem obter uma lista de certificados revogados (CRL)**, clique em **adicionar**. O **adicionar localização** caixa de diálogo é aberta.  
  
5.  Em **adicionar localização**, na **local**, tipo `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`e clique em **Okey**. Isso retorna para a caixa de diálogo de propriedades da autoridade de certificação.  
  
6.  Sobre o **extensões** guia, marque as caixas de seleção a seguir:  
  
    -   **Inclua nas CRLs. Os clientes usam isso para encontrar as listas Delta**  
  
    -   **Incluir na extensão CDP de certificados emitidos**  
  
7.  Em **especificar locais onde os usuários podem obter uma lista de certificados revogados (CRL)**, clique em **adicionar**. O **adicionar localização** caixa de diálogo é aberta.  
  
8.  Em **adicionar localização**, na **local**, tipo `file:\/\/\\\\pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`e clique em **Okey**. Isso retorna para a caixa de diálogo de propriedades da autoridade de certificação.  
  
9. Sobre o **extensões** guia, marque as caixas de seleção a seguir:  
  
    -   **Publicar CRLs para esse local**  
  
    -   **Publicar CRLs Delta para esse local**  
  
10. Alterar **selecione extensão** para **acesso de informações da autoridade (AIA)**e no **especificar locais onde os usuários podem obter uma lista de certificados revogados (CRL)**, faça o seguinte:  
  
    1.  Selecione a entrada que começa com o caminho `ldap:\/\/CN\=<CATruncatedName>,CN\=AIA,CN\=Public Key Services`e clique em **remover**. Em **Confirmar remoção**, clique em **Sim**.  
  
    2.  Selecione a entrada `http:\/\/<ServerDNSName>\/CertEnroll\/<ServerDNSName>\_<CaName><CertificateName>.crt`e clique em **remover**. Em **Confirmar remoção**, clique em **Sim**.  
  
    3.  Selecione a entrada `file:\/\/\\\\<ServerDNSName>\/CertEnroll\/<ServerDNSName>\_<CaName><CertificateName>.crt`e clique em **remover**. Em **Confirmar remoção**, clique em **Sim**.  
  
11. Em **especificar locais onde os usuários podem obter uma lista de certificados revogados (CRL)**, clique em **adicionar**. O **adicionar localização** caixa de diálogo é aberta.  
  
12. Em **adicionar localização**, na **local**, tipo `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`e clique em **Okey**. Isso retorna para a caixa de diálogo de propriedades da autoridade de certificação.  
  
13. Sobre o **extensões**, selecione **incluir no AIA de certificados emitidos**.  
  
14. Em **adicionar localização**, na **local**, tipo `file:\/\/\\\\pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`e clique em **Okey**. Isso retorna para a caixa de diálogo de propriedades da autoridade de certificação.  
  
    > [!IMPORTANT]  
    > Certifique-se de que **incluir na extensão AIA de certificados emitidos** não é selecionado.  
  
15. Quando solicitado a reiniciar os serviços de certificados do Active Directory, clique em **não**. Mais tarde, você reiniciará o serviço.  
  


