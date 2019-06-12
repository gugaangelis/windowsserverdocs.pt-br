---
title: Substituir a Lista de Provedores de Nomes de Domínio
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
ms.openlocfilehash: c087cdad4d2c047db40b370673fb04b232036b61
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433494"
---
# <a name="replace-the-list-of-domain-name-providers"></a>Substituir a Lista de Provedores de Nomes de Domínio

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

É possível substituir a lista de provedores de nomes de domínio exibida no Assistente de Configuração de Nomes de Domínio executando as seguintes tarefas:  


-   [Criar arquivos de serviço de referência](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  

-   [Adicionar uma entrada ao registro no computador de referência](Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

-   [Criar arquivos de serviço de referência](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  

-   [Adicionar uma entrada ao registro no computador de referência](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  


###  <a name="BKMK_ReferralFiles"></a> Criar arquivos de serviço de referência  
 A Ferramenta de Administração de Serviços de Referência cria um conjunto de arquivos usados para definir a lista de provedores de nomes de domínio exibidos no Assistente de Configuração de Nomes de Domínio. Um arquivo XML formatado é criado para cada região do mundo e contém informações sobre os provedores de nomes de domínio que você especificou na ferramenta. Os arquivos criados pela ferramenta devem estar localizados em uma pasta que possa ser acessada por meio de um link seguro (HTTPS) que você gerencia pela Internet.  

##### <a name="to-create-the-referral-files"></a>Para criar os arquivos de referência  

1.  Abra a Ferramenta de Administração de Serviços de Referência.  

2.  Clique em **Adicionar**.  

3.  Na caixa de diálogo Adicionar Provedor de Nomes de Domínio, digite o nome do provedor de nomes de domínio.  

4.  Adicione os domínios de nível superior que são suportados pelo provedor de nomes de domínio. Isso é possível clicando em **Adicionar**, digitando o identificador do domínio de nível superior e selecionando as regiões compatíveis. Você pode selecionar **Todas as Regiões**.  

5.  Digite a descrição do provedor de nomes de domínio.  

6.  Adicione as URLs de todos os sites associados ao provedor de nomes de domínio.  

7.  Se um logotipo estiver disponível para o provedor de nomes de domínio, adicione-o clicando em **Alterar Logotipo**.  

8.  Clique em **Salvar**.  

9. Repita as etapas 2 a 8 para cada provedor de nomes de domínio que desejar listar no assistente.  

10. Depois de adicionar todos os provedores de nome de domínio, escolha a pasta onde os arquivos de referência estará localizados. Ao escolher uma pasta, lembre-se de que os arquivos de referência devem ser acessados por meio de um link HTTPS.  

11. Clique em **Gerar Arquivos para o Sistema de Arquivos**.  

###  <a name="BKMK_AddRegistry"></a> Adicionar uma entrada ao registro no computador de referência  
 Uma entrada de Registro deve ser adicionada para que se especifique onde o sistema operacional pode achar os arquivos de serviços de referência.  

##### <a name="to-add-a-key-to-the-registry"></a>Para adicionar uma chave ao Registro  

1.  No computador de referência, clique em **Iniciar**, insira **regedit**e pressione **Enter**.  

2.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**, expanda **Gerenciadores de Domínio** e, finalmente, expanda **Provedores**.  

3.  Clique com o botão direito do mouse na chave **E423C85D-6B1F-4583-95E0-449D8263BAC4** e, em seguida, clique em **Valor da Cadeia de Caracteres**.  

4.  Digite **ReferralServerHttpsUri** para o nome da cadeia de caracteres e pressione **Enter**.  

5.  Clique com o botão direito na nova cadeia de caracteres **ReferralServerHttpsUri** no painel à direita e, em seguida, clique em **Modificar**.  


6.  Digite a URL HTTPS usada para acessar os arquivos de referência criados em [Create the referral service files](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles), e clique em **OK**.  

6.  Digite a URL HTTPS usada para acessar os arquivos de referência criados em [Create the referral service files](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles), e clique em **OK**.  


~~~
> [!IMPORTANT]
>  A slash (/) is required at the end of the URL.  
~~~

###  <a name="BKMK_ReplaceDomainNameProviders"></a> Problemas de status do nome de domínio  
 Se um parceiro adiciona provedores de nome de domínio e usa uma interface de programação de aplicativo (API) no SDK do Windows Server Essentials para definir os status desconhecido, falha e CertificateRequestNotSubmitted para o certificado, o cliente recebe um incorreto resultado da mensagem e a configuração. Isso ocorre porque os casos são tratados por exceções, em vez de retornarem um status.  

 Os seguintes status de domínio são falhas e devem ser relatados como erro:  

- Failed (Falha)  

- PendingCustomerInterventionRequired (Intervenção de cliente pendente requerida)  

- PurchaseFailed (Falha na compra)  

- DomainNotFound (Domínio não encontrado)  

- InRenewalCustomerInterventionRequired (Intervenção de cliente requerida na renovação)  

- RenewalFailed (Falha na renovação)  

  Os seguintes status de domínio são bem-sucedidos e devem ser relatados como sucesso:  

- Pronto  

- Pending (Pendente)  

- InRenewal (Em renovação)  

## <a name="see-also"></a>Consulte também  

 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](Testing-the-Customer-Experience.md)

 [Criando e personalizando a imagem](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](../install/Additional-Customizations.md)   
 [Preparando a imagem para implantação](../install/Preparing-the-Image-for-Deployment.md)   
 [Testar a experiência do usuário](../install/Testing-the-Customer-Experience.md)

