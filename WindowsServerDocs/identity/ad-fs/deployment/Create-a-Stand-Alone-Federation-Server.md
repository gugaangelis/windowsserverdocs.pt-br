---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Criar um servidor de federação autônomo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3a948fadeb1cfa2ebe259a9bb659e93a85e06910
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359662"
---
# <a name="create-a-stand-alone-federation-server"></a>Criar um servidor de federação autônomo

Depois de instalar o Serviço de Federação serviço de função e configurar os certificados necessários em um computador, você estará pronto para configurar o computador para se tornar um servidor de Federação. Você pode usar o procedimento a seguir para configurar o computador para se tornar um servidor de Federação do stand @ no__t-0alone. O ato de criar um servidor de Federação do stand @ no__t-0alone também cria um novo Serviço de Federação. Você cria um servidor de Federação com o assistente de configuração do servidor de Federação AD FS.  
  
> [!NOTE]  
> Para o design da Web federado @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3, você deve ter pelo menos um servidor de Federação na organização do parceiro de conta e pelo menos um servidor de Federação na organização do parceiro de recurso. Para obter mais informações, consulte [Onde colocar um servidor de federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examinar os detalhes sobre como usar as contas apropriadas e as associações de grupo em \( [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) http:\/\/go.Microsoft.com\/fwlink\/? \=LinkId 83477.\)   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Para criar um servidor de Federação do stand @ no__t-0alone  
  
1.  Há duas maneiras de iniciar o assistente de configuração do servidor de Federação AD FS. Para iniciar o assistente, tome uma das seguintes ações:  
  
    -   Depois que a instalação do serviço de função Serviço de Federação for concluída, abra o AD FS Management snap @ no__t-0in e clique no link do **Assistente de configuração do servidor de federação AD FS** na página **visão geral** ou no painel **ações** .  
  
    -   A qualquer momento depois que o assistente de instalação for concluído, abra o Windows Explorer, navegue até a pasta **C: \\Windows @ no__t-2ADFS** e, em seguida, clique em @ no__t-3Click **FsConfigWizard. exe**.  
  
2.  Na página **Bem-vindo**, verifique se **Criar um novo Serviço de Federação** está selecionado e clique em **Avançar**.  
  
3.  Na página de **implantação selecionar stand @ no__t-1Alone ou farm** , clique em **stand @ no__t-3alone Federation Server**e, em seguida, clique em **Avançar**.  
  
    > [!IMPORTANT]  
    > Quando você seleciona a opção de servidor de Federação stand @ no__t-0alone no assistente de configuração do servidor de Federação AD FS, a conta de serviço associada a esse Serviço de Federação será automaticamente atribuída à conta de serviço de rede. O uso do serviço de rede como a conta de serviço só é recomendado em situações em que você esteja avaliando AD FS em um ambiente de laboratório de teste. Se você pretende usar a opção de servidor de Federação stand @ no__t-0alone para implantar um servidor de Federação em um ambiente de produção, é importante que você altere essa conta de serviço para uma conta de serviço mais apropriada que possa ser dedicada a servir solicitações para Esse novo Serviço de Federação. Alterar a conta de serviço para uma conta diferente do serviço de rede reduzirá possíveis vetores de ataque que, de outra forma, tornaria o servidor de Federação vulnerável a ataques mal-intencionados.  
  
4.  Na página **Especificar o Nome do Serviço de Federação**, verifique se o **Certificado SSL** que aparece está correto. Caso contrário, selecione o certificado apropriado na lista **certificado SSL** .  
  
    Esse certificado é gerado a partir das configurações protocolo SSL \(SSL @ no__t-1 para o site padrão. Se o Site Padrão tiver somente um certificado SSL configurado, esse certificado será apresentado e automaticamente selecionado para uso. Se vários certificados SSL forem configurados para o Site Padrão, todos esses certificados serão listados aqui e você deverá selecionar um dentre eles. Se não houver configurações do SSL definidas para o Site Padrão, a lista será gerada dos certificados disponíveis no repositório de certificados pessoal no computador local.  
  
    > [!NOTE]  
    > O assistente não permitirá que você substitua o certificado se um certificado SSL estiver configurado para IIS. Isso assegura que qualquer configuração anterior do IIS pretendida para certificados SSL seja preservada. Para contornar essa restrição, você pode remover o certificado ou reconfigurá-lo manualmente com o console de gerenciamento do IIS.  
  
5.  Se o banco de dados de AD FS que você selecionou já existir, a página **banco de dados de configuração AD FS existente detectado** será exibida. Se isso ocorrer, clique em **Excluir banco de dados** e clique em **Avançar**.  
  
    > [!CAUTION]  
    > Selecione esta opção somente quando você tiver certeza de que os dados nesse banco de dado AD FS não são importantes ou que não são usados em um farm de servidores de Federação de produção.  
  
6.  Na página **Pronto para Aplicar Configurações**, examine os detalhes. Se as configurações parecerem corretas, clique em **Avançar** para começar a configurar AD FS com essas configurações.  
  
7.  Na página **Resultados da Configuração**, examine os resultados. Quando todas as etapas de configuração forem concluídas, clique em **fechar** para sair do assistente.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  

