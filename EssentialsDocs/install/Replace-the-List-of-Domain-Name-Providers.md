---
title: Substituir a Lista de Provedores de Nomes de Domínio
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 104d0412-2d77-4cd4-99f7-65a885522850
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 74509a7d64e718fe1d2b62f806306235e7d827e4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623361"
---
# <a name="replace-the-list-of-domain-name-providers"></a>Substituir a Lista de Provedores de Nomes de Domínio

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

É possível substituir a lista de provedores de nomes de domínio exibida no Assistente de Configuração de Nomes de Domínio executando as seguintes tarefas:


-   [Criar arquivos de serviços de referência](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)

-   [Adicionar uma entrada no Registro do computador de referência](Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)

-   [Criar arquivos de serviços de referência](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)

-   [Adicionar uma entrada no Registro do computador de referência](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)


###  <a name="create-the-referral-service-files"></a><a name="BKMK_ReferralFiles"></a> Criar os arquivos do serviço de referência
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

10. Depois que tiver adicionado todos os provedores de nomes de domínio, escolha a pasta em que os arquivos de referência ficarão localizados. Ao escolher uma pasta, lembre-se de que os arquivos de referência devem ser acessados por meio de um link HTTPS.

11. Clique em **Gerar Arquivos para o Sistema de Arquivos**.

###  <a name="add-an-entry-to-the-registry-on-the-reference-computer"></a><a name="BKMK_AddRegistry"></a> Adicionar uma entrada ao registro no computador de referência
 Uma entrada de Registro deve ser adicionada para que se especifique onde o sistema operacional pode achar os arquivos de serviços de referência.

##### <a name="to-add-a-key-to-the-registry"></a>Para adicionar uma chave ao Registro

1.  No computador de referência, clique em **Iniciar**, digite **regedit** e pressione **Enter**.

2.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**, expanda **Gerenciadores de Domínio** e, finalmente, expanda **Provedores**.

3.  Clique com o botão direito do mouse na chave **E423C85D-6B1F-4583-95E0-449D8263BAC4** e, em seguida, clique em **Valor da Cadeia de Caracteres**.

4.  Digite **ReferralServerHttpsUri** para o nome da cadeia de caracteres e pressione **Enter**.

5.  Clique com o botão direito na nova cadeia de caracteres **ReferralServerHttpsUri** no painel à direita e, em seguida, clique em **Modificar**.


6.  Digite a URL HTTPS usada para acessar os arquivos de referência criados em [Criar arquivos de serviços de referência](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles) e clique em **OK**.

6.  Digite a URL HTTPS usada para acessar os arquivos de referência criados em [Criar arquivos de serviços de referência](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles) e clique em **OK**.


~~~
> [!IMPORTANT]
>  A slash (/) is required at the end of the URL.
~~~

###  <a name="domain-name-status-issues"></a><a name="BKMK_ReplaceDomainNameProviders"></a> Problemas de status do nome de domínio
 Se um parceiro adicionar provedores de nome de domínio e usar uma API (interface de programação de aplicativo) no SDK do Windows Server Essentials para definir os status desconhecido, falha e CertificateRequestNotSubmitted para o certificado, o cliente receberá uma mensagem incorreta e o resultado da configuração. Isso ocorre porque os casos são tratados por exceções, em vez de retornarem um status.

 Os seguintes status de domínio são falhas e devem ser relatados como erro:

- Failed (Falha)

- PendingCustomerInterventionRequired (Intervenção de cliente pendente requerida)

- PurchaseFailed (Falha na compra)

- DomainNotFound (Domínio não encontrado)

- InRenewalCustomerInterventionRequired (Intervenção de cliente requerida na renovação)

- RenewalFailed (Falha na renovação)

  Os seguintes status de domínio são bem-sucedidos e devem ser relatados como sucesso:

- Ready

- Pending (Pendente)

- InRenewal (Em renovação)

## <a name="see-also"></a>Consulte Também

 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)

