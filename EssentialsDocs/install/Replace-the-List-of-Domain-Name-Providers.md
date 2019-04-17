---
title: "Substituir a lista de provedores de nome de domínio"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 104d0412-2d77-4cd4-99f7-65a885522850
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ebaef0f88456f61fa229c9a18ee8014987fe7fa7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="replace-the-list-of-domain-name-providers"></a>Substituir a lista de provedores de nome de domínio

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode substituir a lista de provedores de nome de domínio que é exibida no Assistente de configuração de nome de domínio, concluindo as seguintes tarefas:  
  

-   [Criar arquivos de serviço de referência](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [Adicione uma entrada ao registro no computador de referência](Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

-   [Criar arquivos de serviço de referência](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [Adicione uma entrada ao registro no computador de referência](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

  
###  <a name="BKMK_ReferralFiles"></a>Criar arquivos de serviço de referência  
 A ferramenta de administração de serviço de referência cria um conjunto de arquivos que são usados para definir a lista de provedores de nome de domínio que são exibidos no Assistente de configuração de nome de domínio. Um arquivo em formato XML é criado para cada região do mundo e contém informações para os provedores de nome de domínio que você especifica na ferramenta. Os arquivos que são criados pela ferramenta devem estar localizados em uma pasta que pode ser acessada por meio de um link seguro (HTTPS) que você gerencia na Internet.  
  
##### <a name="to-create-the-referral-files"></a>Para criar os arquivos de referência  
  
1.  Abra a ferramenta de administração de serviço de referência.  
  
2.  Clique em **adicionar**.  
  
3.  Em Adicionar uma caixa de diálogo do provedor de nomes de domínio, digite o nome do provedor de nomes de domínio.  
  
4.  Adicione domínios de nível superior que são compatíveis com o provedor de nomes de domínio. Você pode fazer isso, clicando em **adicionar**, inserir o identificador de domínio de nível superior e selecione as regiões com suporte. Você pode selecionar **todas as regiões**.  
  
5.  Insira a descrição do provedor de nomes de domínio.  
  
6.  Adicione as URLs para todos os sites que estão associados com o provedor de nomes de domínio.  
  
7.  Se um logotipo estiver disponível para o provedor de nomes de domínio, adicione o logotipo clicando **alterar logotipo**.  
  
8.  Clique em **salvar**.  
  
9. Repita as etapas 2 a 8 para cada provedor de nomes de domínio que você deseja listar no assistente.  
  
10. Depois de adicionar todos os provedores de nome de domínio, escolha a pasta onde os arquivos de referência estarão localizados. Tenha em mente ao escolher uma pasta que os arquivos de referência devem ser acessados por meio de um link HTTPS.  
  
11. Clique em **gerar arquivos para sistema de arquivos**.  
  
###  <a name="BKMK_AddRegistry"></a>Adicione uma entrada ao registro no computador de referência  
 Uma entrada do registro deve ser adicionada para especificar onde o sistema operacional pode encontrar a referência arquivos do serviço.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Para adicionar uma chave do registro  
  
1.  No computador de referência, clique em **iniciar**, insira **regedit**e pressione **Enter**.  
  
2.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**, expanda **gerenciadores de domínio**e, em seguida, expanda **provedores**.  
  
3.  Clique com botão direito do **E423C85D-6B1F-4583-95E0-449D8263BAC4** chave e clique em **valor de cadeia de caracteres**.  
  
4.  Tipo **ReferralServerHttpsUri** para o nome da cadeia de caracteres e pressione **Enter**.  
  
5.  Clique com botão direito do novo **ReferralServerHttpsUri** cadeia de caracteres no painel direito e clique em **modificar**.  
  

6.  Digite a URL HTTPS que é usado para acessar os arquivos de referência que você criou na [criar arquivos de serviço de referência](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)e clique em **Okey**.  

6.  Digite a URL HTTPS que é usado para acessar os arquivos de referência que você criou na [criar arquivos de serviço de referência](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)e clique em **Okey**.  

  
    > [!IMPORTANT]
    >  Uma barra (/) é necessária ao final da URL.  
  
###  <a name="BKMK_ReplaceDomainNameProviders"></a>Problemas de status de nome de domínio  
 Se um parceiro adiciona provedores de nome de domínio e usa uma interface de programação de aplicativo (API) no SDK do Windows Server Essentials para definir os status Unknown, Failed e CertificateRequestNotSubmitted para o certificado, o cliente recebe uma mensagem incorreta e um resultado de configuração. Isso ocorre porque os casos são tratados por exceções em vez de retornar um status.  
  
 Os seguintes status de domínio são falhas e devem ser relatados como erros:  
  
-   Falhou  
  
-   PendingCustomerInterventionRequired  
  
-   PurchaseFailed  
  
-   DomainNotFound  
  
-   InRenewalCustomerInterventionRequired  
  
-   RenewalFailed  
  
 Os seguintes status de domínio são bem-sucedidos e devem ser relatados como êxito:  
  
-   Pronto  
  
-   Pendente  
  
-   InRenewal  
  
## <a name="see-also"></a>Consulte também  

 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)

 [Criar e personalizar a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](../install/Testing-the-Customer-Experience.md)

