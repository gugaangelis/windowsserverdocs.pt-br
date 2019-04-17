---
title: Instale uma autoridade de certificação
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17a887ab32570b739d5ca99611ee0d496a966d1b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-certification-authority"></a>Instale uma autoridade de certificação

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para instalar os serviços de certificados do Active Directory (AD CS) para que você pode registrar um certificado de servidor para servidores que executam o servidor de política de rede (NPS), roteamento e acesso remoto (RRAS) ou ambos.  
  
> [!IMPORTANT]  
> -   Antes de instalar os serviços de certificados do Active Directory, você deve nomear o computador, configurar o computador com um endereço IP estático e ingressar o computador ao domínio. Para obter mais informações sobre como realizar essas tarefas, consulte o Windows Server 2016 [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).  
> -   Para executar este procedimento, o computador no qual você está instalando o AD CS deve ter ingressado em um domínio em que os serviços de domínio do Active Directory (AD DS) é instalado.  
  
A associação ao grupo ambos **administradores corporativos** e o domínio raiz **Admins. do domínio** grupo é o requisito mínimo para concluir este procedimento.  
  
> [!NOTE]  
> Para executar este procedimento usando o Windows PowerShell, abra o Windows PowerShell, digite o seguinte comando e pressione ENTER.   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> Após a instalação do AD CS, digite o seguinte comando e pressione ENTER.  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>Para instalar os serviços de certificados do Active Directory  
  
1.  Faça logon como um membro do grupo Administradores de empresa e a raiz do grupo Admins.  
  
2.  No Gerenciador do servidor, clique em **gerenciar**e clique em **adicionar funções e recursos**. Abre o assistente Adicionar funções e recursos.  
  
3.  Em **Before You Begin**, clique em **próxima**.  
  
    > [!NOTE]  
    > O **Before You Begin** página do Assistente de recursos de adição de funções e não é exibida se você tiver selecionado anteriormente **ignorar esta página por padrão** quando a adição de funções e recursos de assistente foi executado.  
  
4.  Em **selecione tipo de instalação**, certifique-se de que **instalação baseada em função ou recurso baseado** está selecionado e clique em **próxima**.  
  
5.  Em **servidor de destino Select**, certifique-se de que **selecionar um servidor do pool servidor** é selecionado. Em **Pool de servidores**, verifique se o computador local está selecionado. Clique em **próxima**.  
  
6.  Em **selecionar funções de servidor**, na **funções**, selecione **serviços de certificados do Active Directory**. Quando você for solicitado a adicionar recursos necessários, clique em **adicionar recursos**e clique em **próxima**.  
  
7.  Em **Selecione recursos**, clique em **próxima**.  
  
8.  Em **serviços de certificados do Active Directory**, leia as informações fornecidas e, em seguida, clique em **próxima**.  
  
9. Em **confirmar seleções de instalação**, clique em **instalar**. Não feche o assistente durante o processo de instalação. Quando a instalação for concluída, clique em **configurar os serviços de certificados do Active Directory no servidor de destino**. O Assistente de configuração do AD CS é aberta. Leia as informações de credenciais e, se necessário, forneça as credenciais para uma conta que seja um membro do grupo Administradores corporativos. Clique em **próxima**.  
  
10. Em **serviços de função**, clique em **autoridade de certificação**e clique em **próxima**.  
  
11. No **o tipo de instalação** página, verifique **autoridade de certificação corporativa** está selecionado e clique em **próxima**.  
  
12. No **Especifica o tipo da autoridade de certificação** página, verifique **CA raiz** está selecionado e clique em **próxima**.  
  
13. No **especificam o tipo de chave privada** página, verifique **criar uma nova chave privada** está selecionado e clique em **próxima**.  
  
14. No **criptografia para CA** página, mantenha as configurações padrão de CSP (**provedor de armazenamento de chaves de Software do RSA #Microsoft**) e o algoritmo de hash (**SHA1**) e determinar o tamanho de caractere da tecla recomendado para a implantação. Tamanhos de caractere da tecla grandes fornecem segurança ideal; No entanto, eles podem afetar o desempenho de servidor e podem não ser compatíveis com aplicativos herdados. É recomendável que você mantenha a configuração padrão de 2048. Clique em **próxima**.  
  
15. Sobre o **nome da CA** página, mantenha o nome comum sugerido para a autoridade de certificação ou altere o nome de acordo com suas necessidades. Certifique-se de que você tiver certeza de que o nome da CA é compatível com seu convenções de nomenclatura e finalidades, porque você não pode alterar o nome de autoridade de certificação depois que você instalou o AD CS. Clique em **próxima**.  
  
16. Sobre o **período de validade** página **Especifica o período de validade**, digite o número e selecione um valor de hora (anos, meses, semanas ou dias). Recomenda-se a configuração padrão de cinco anos. Clique em **próxima**.  
  
17. No **banco de dados de CA** página **especificar os locais de banco de dados**, especifique o local da pasta para o banco de dados de certificado e o log de banco de dados de certificado. Se você especificar locais diferentes dos locais padrão, certifique-se de que as pastas estão protegidas com listas de controle de acesso (ACLs) que impedem que os usuários não autorizados ou computadores acessem os CA banco de dados e arquivos de log. Clique em **próxima**.  
  
18. Em **confirmação**, clique em **configurar** para aplicar as seleções e clique em **fechar**.  
  


