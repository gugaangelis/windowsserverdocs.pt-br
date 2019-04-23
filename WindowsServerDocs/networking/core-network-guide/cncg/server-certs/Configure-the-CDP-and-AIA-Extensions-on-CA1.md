---
title: Configurar as extensões CDP e AIA em CA1
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: dougkim
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/26/2018
ms.openlocfilehash: 2bdd03636d1cbbcf5b0355358cbedeb63f97af1b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834217"
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>Configurar as extensões CDP e AIA em CA1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para configurar o ponto de distribuição da lista de revogação de certificados (CRL) (CDP) e as configurações de acesso de informações da autoridade (AIA) em CA1.  
  
Para executar este procedimento, você deve ser um membro do grupo Administradores de domínio.  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>Para configurar as extensões AIA e CDP em CA1  
  
1.  No Gerenciador do Servidor, clique em **Ferramentas** e depois em **Autoridade de Certificação**.  
  
2.  Na árvore de console autoridade de certificação, clique com botão direito **corp-CA1-CA**e, em seguida, clique em **propriedades**.  
  
    > [!NOTE]  
    > O nome da autoridade de certificação é diferente, se você não nomear o computador CA1 e seu nome de domínio é diferente daquele que neste exemplo. O nome da autoridade de certificação está no formato *domínio*-*CAComputerName*- CA.  
  
3.  Clique na guia **Extensões**. Certifique-se de que **Selecionar extensão** é definido como **ponto de distribuição da CRL (CDP)** e, nas **Especifique locais onde os usuários podem obter uma lista de certificados revogados (CRL)**, Faça o seguinte:  
  
    1.  Selecione a entrada `file://\\<ServerDNSName>\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`e, em seguida, clique em **remover**. Na **Confirmar remoção**, clique em **Sim**.  
  
    2.  Selecione a entrada `https://<ServerDNSName>/CertEnroll/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`e, em seguida, clique em **remover**. Na **Confirmar remoção**, clique em **Sim**.  
  
    3.  Selecione a entrada que começa com o caminho `ldap:///CN=<CATruncatedName><CRLNameSuffix>,CN=<ServerShortName>`e, em seguida, clique em **remover**. Na **Confirmar remoção**, clique em **Sim**.  
  
4.  Na **especificar locais dos quais os usuários podem obter uma lista de certificados revogados (CRL)**, clique em **Add**. O **adicionar local** caixa de diálogo é aberta.  
  
5.  Na **adicionar local**, na **local**, digite `https://pki.corp.contoso.com/pki/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`e, em seguida, clique em **Okey**. Isso retorna à caixa de diálogo de propriedades de autoridade de certificação.  
  
6.  Sobre o **extensões** guia, marque as caixas de seleção a seguir:  
  
    -   **Inclua em CRLs. Os clientes usam isso para encontrar os locais de CRL Delta**  
  
    -   **Incluir na extensão CDP de certificados emitidos**  
  
7.  Na **especificar locais dos quais os usuários podem obter uma lista de certificados revogados (CRL)**, clique em **Add**. O **adicionar local** caixa de diálogo é aberta.  
  
8.  Na **adicionar local**, na **local**, digite `file://\\pki.corp.contoso.com\pki\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`e, em seguida, clique em **Okey**. Isso retorna à caixa de diálogo de propriedades de autoridade de certificação.  
  
9. Sobre o **extensões** guia, marque as caixas de seleção a seguir:  
  
    -   **Publicar CRLs neste local**  
  
    -   **Publicar CRLs Delta neste local**  
  
10. Alteração **Selecionar extensão** ao **acesso de informações da autoridade (AIA)** e, nas **Especifique locais onde os usuários podem obter uma lista de certificados revogados (CRL)**, fazer o seguinte:  
  
    1.  Selecione a entrada que começa com o caminho `ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services`e, em seguida, clique em **remover**. Na **Confirmar remoção**, clique em **Sim**.  
  
    2.  Selecione a entrada `https://<ServerDNSName>/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt`e, em seguida, clique em **remover**. Na **Confirmar remoção**, clique em **Sim**.  
  
    3.  Selecione a entrada `file://\\<ServerDNSName>\CertEnroll\<ServerDNSName><CaName><CertificateName>.crt`e, em seguida, clique em **remover**. Na **Confirmar remoção**, clique em **Sim**.  
  
11. Na **especificar locais dos quais os usuários podem obter o certificado da autoridade de certificação**, clique em **Add**. O **adicionar local** caixa de diálogo é aberta.  
  
12. Na **adicionar local**, na **local**, digite `https://pki.corp.contoso.com/pki/<ServerDNSName>_<CaName><CertificateName>.crt`e, em seguida, clique em **Okey**. Isso retorna à caixa de diálogo de propriedades de autoridade de certificação.  
  
13. Sobre o **extensões** guia, selecione **incluir o AIA dos certificados emitidos**.  
  
14. Quando solicitado a reiniciar os serviços de certificados do Active Directory, clique em **não**. Você reiniciará o serviço mais tarde.  
  

