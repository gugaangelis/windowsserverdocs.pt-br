---
title: Instalar a autoridade de certificação
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1774d235703bd75d810f2649cb8ed3f2f92622d5
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811600"
---
# <a name="install-the-certification-authority"></a>Instalar a autoridade de certificação

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para instalar os serviços de certificados do Active Directory (AD CS), de modo que você pode registrar um certificado de servidor para servidores que executam o servidor de diretivas de rede (NPS), roteamento e acesso remoto (RRAS) ou ambos.  
  
> [!IMPORTANT]  
> -   Antes de instalar os serviços de certificados do Active Directory, nome do computador, configure o computador com um endereço IP estático e ingressar o computador no domínio. Para obter mais informações sobre como realizar essas tarefas, consulte o Windows Server 2016 [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).  
> -   Para executar este procedimento, o computador no qual você está instalando o AD CS deve ser associado a um domínio em que os serviços de domínio do Active Directory (AD DS) está instalado.  
  
A associação aos grupos **Administradores de Empresa** e **Admins. do Domínio** do domínio raiz é o mínimo exigido para executar este procedimento.  
  
> [!NOTE]  
> Para executar esse procedimento usando o Windows PowerShell, abra o Windows PowerShell e digite o seguinte comando e pressione ENTER.   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> Depois que o AD CS é instalado, digite o seguinte comando e pressione ENTER.  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>Para instalar os Serviços de Certificados do Active Directory  

> [!TIP]
> Se você quiser usar o Windows PowerShell para instalar os serviços de certificados do Active Directory, consulte [Install-AdcsCertificationAuthority](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcscertificationauthority?view=win10-ps) para cmdlets e parâmetros opcionais.
  
1.  Faça logon como membro do grupo Administradores de Empresa e do grupo Admins. do Domínio do domínio raiz.  
  
2.  No Gerenciador do Servidor, clique em **Gerenciar**e depois em **Adicionar Funções e Recursos**. O Assistente para Adicionar Funções e Recursos é aberto.  
  
3.  Em **Antes de Começar**, clique em **Avançar**.  
  
    > [!NOTE]  
    > A página **Antes de Começar** do Assistente para Adicionar Funções e Recursos não é exibida quando você marca a opção **Ignorar esta página por padrão** quando o Assistente para Funções e Recursos foi executado.  
  
4.  Em **Selecionar Tipo de Instalação**, verifique se **Instalação baseada em função ou recurso** está marcada e clique em **Avançar**.  
  
5.  Em **Selecionar servidor de destino**, verifique se **Selecionar um servidor no pool de servidores** está marcada. Em **Pool de Servidores**, verifique se o computador local está selecionado. Clique em **Avançar**.  
  
6.  Na **selecionar funções do servidor**, na **funções**, selecione **serviços de certificados do Active Directory**. Quando você for solicitado a adicionar recursos necessários, clique em **adicionar recursos**e, em seguida, clique em **próxima**.  
  
7.  Na **selecionar recursos**, clique em **próxima**.  
  
8.  Na **serviços de certificados do Active Directory**, leia as informações fornecidas e, em seguida, clique em **próxima**.  
  
9. Em **Confirmar seleções de instalação**, clique em **Instalar**. Não feche o assistente durante o processo de instalação. Quando a instalação for concluída, clique em **configurar os serviços de certificados do Active Directory no servidor de destino**. Abre o Assistente de configuração do AD CS. Leia as informações de credenciais e, se necessário, forneça as credenciais para uma conta que seja membro do grupo Administradores corporativos. Clique em **Avançar**.  
  
10. Na **serviços de função**, clique em **autoridade de certificação**e, em seguida, clique em **próxima**.  
  
11. Sobre o **o tipo de instalação** página, verifique **autoridade de certificação corporativa** está selecionado e, em seguida, clique em **próxima**.  
  
12. Sobre o **especificar o tipo da autoridade de certificação** página, verifique **autoridade de certificação raiz** está selecionado e, em seguida, clique em **próxima**.  
  
13. Sobre o **especificar o tipo da chave privada** página, verifique **criar uma nova chave privada** está selecionado e, em seguida, clique em **próxima**.  
  
14. Sobre o **criptografia para AC** página, mantenha as configurações padrão para o CSP (**RSA #Microsoft Software Key Storage Provider**) e o algoritmo de hash (**SHA2**) e determinar o melhor comprimento de chave para sua implantação. Tamanhos chaves grandes proporcionam a segurança ideal; No entanto, eles podem afetar o desempenho do servidor e podem não ser compatíveis com aplicativos herdados. É recomendável que você mantenha a configuração padrão de 2048. Clique em **Avançar**.  
  
15. Sobre o **nome da autoridade de certificação** página, mantenha o nome comum sugerido para a autoridade de certificação ou altere o nome de acordo com suas necessidades. Certifique-se de que você tiver certeza de que o nome da autoridade de certificação é compatível com suas convenções de nomenclatura e algumas finalidades, porque você não pode alterar o nome da autoridade de certificação depois de instalar o AD CS. Clique em **Avançar**.  
  
16. Sobre o **período de validade** página, na **especifique o período de validade**, digite o número e selecione um valor de tempo (anos, meses, semanas ou dias). A configuração padrão de cinco anos é recomendada. Clique em **Avançar**.  
  
17. Sobre o **banco de dados de autoridade de certificação** página, na **especifique os locais do banco de dados**, especifique o local de pasta para o banco de dados do certificado e o log de banco de dados do certificado. Se você especificar localizações diferentes do padrão, verifique se as pastas estão protegidas com ACLs (listas de controle de acesso) que impedem que usuários ou computadores não autorizados acessem os arquivos de log e o banco de dados da autoridade de certificação. Clique em **Avançar**.  
  
18. Na **confirmação**, clique em **configurar** para aplicar suas seleções e, em seguida, clique em **fechar**.  
  


