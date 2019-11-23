---
title: Configurar as extensões CDP e AIA em CA1
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: dougkim
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/26/2018
ms.openlocfilehash: 94304f6ae2604dad9f1d21be62d19e4a57a7a1ae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356173"
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>Configurar as extensões CDP e AIA em CA1

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para configurar o CDP (ponto de distribuição) da CRL (lista de certificados revogados) e as configurações de AIA (acesso a informações de autoridade) em CA1.  
  
Para executar esse procedimento, você deve ser membro de admins. do domínio.  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>Para configurar as extensões de CDP e AIA no CA1  
  
1.  No Gerenciador do Servidor, clique em **Ferramentas** e depois em **Autoridade de Certificação**.  
  
2.  Na árvore de console da autoridade de certificação, clique com o botão direito do mouse em **Corp-CA1-AC**e clique em **Propriedades**.  
  
    > [!NOTE]  
    > O nome da sua autoridade de certificação será diferente se você não nomear o computador CA1 e o nome de domínio for diferente daquele neste exemplo. O nome da autoridade de certificação está no formato *domínio*-*CAComputerName*-ca.  
  
3.  Clique na guia **extensões** . Verifique se a **extensão selecionada** está definida como **CDP (ponto de distribuição de CRL)** e, em **especificar locais dos quais os usuários podem obter uma CRL (lista de certificados**revogados), faça o seguinte:  
  
    1.  Selecione a entrada `file://\\<ServerDNSName>\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`e, em seguida, clique em **remover**. Em **confirmar remoção**, clique em **Sim**.  
  
    2.  Selecione a entrada `http://<ServerDNSName>/CertEnroll/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`e, em seguida, clique em **remover**. Em **confirmar remoção**, clique em **Sim**.  
  
    3.  Selecione a entrada que começa com o caminho `ldap:///CN=<CATruncatedName><CRLNameSuffix>,CN=<ServerShortName>`e, em seguida, clique em **remover**. Em **confirmar remoção**, clique em **Sim**.  
  
4.  Em **especificar locais dos quais os usuários podem obter uma CRL (lista de certificados revogados)** , clique em **Adicionar**. A caixa de diálogo **Adicionar local** é aberta.  
  
5.  Em **Adicionar local**, em **local**, digite `http://pki.corp.contoso.com/pki/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`e clique em **OK**. Isso retornará para a caixa de diálogo Propriedades da autoridade de certificação.  
  
6.  Na guia **extensões** , marque as seguintes caixas de seleção:  
  
    -   **Incluir em CRLs. os clientes usam isso para localizar os locais de CRL delta**  
  
    -   **Incluir na extensão de CDP de certificados emitidos**  
  
7.  Em **especificar locais dos quais os usuários podem obter uma CRL (lista de certificados revogados)** , clique em **Adicionar**. A caixa de diálogo **Adicionar local** é aberta.  
  
8.  Em **Adicionar local**, em **local**, digite `file://\\pki.corp.contoso.com\pki\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`e clique em **OK**. Isso retornará para a caixa de diálogo Propriedades da autoridade de certificação.  
  
9. Na guia **extensões** , marque as seguintes caixas de seleção:  
  
    -   **Publicar CRLs neste local**  
  
    -   **Publicar CRLs delta neste local**  
  
10. Altere **selecionar extensão** para **acesso a informações de autoridade (AIA)** e, em **especificar locais dos quais os usuários podem obter uma CRL (lista de certificados revogados)** , faça o seguinte:  
  
    1.  Selecione a entrada que começa com o caminho `ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services`e, em seguida, clique em **remover**. Em **confirmar remoção**, clique em **Sim**.  
  
    2.  Selecione a entrada `http://<ServerDNSName>/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt`e, em seguida, clique em **remover**. Em **confirmar remoção**, clique em **Sim**.  
  
    3.  Selecione a entrada `file://\\<ServerDNSName>\CertEnroll\<ServerDNSName><CaName><CertificateName>.crt`e, em seguida, clique em **remover**. Em **confirmar remoção**, clique em **Sim**.  
  
11. Em **especificar locais dos quais os usuários podem obter o certificado para essa autoridade de certificação**, clique em **Adicionar**. A caixa de diálogo **Adicionar local** é aberta.  
  
12. Em **Adicionar local**, em **local**, digite `http://pki.corp.contoso.com/pki/<ServerDNSName>_<CaName><CertificateName>.crt`e clique em **OK**. Isso retornará para a caixa de diálogo Propriedades da autoridade de certificação.  
  
13. Na guia **extensões** , selecione **incluir no AIA de certificados emitidos**.  
  
14. Quando for solicitado a reiniciar Active Directory serviços de certificados, clique em **não**. Você reiniciará o serviço mais tarde.  
  

