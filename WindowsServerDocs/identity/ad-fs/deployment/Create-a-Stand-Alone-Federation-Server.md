---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Criar um servidor de federação autônomo
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4b70b0b048f66f9a8ba19cd7990dde57e0655ae4
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192234"
---
# <a name="create-a-stand-alone-federation-server"></a>Criar um servidor de federação autônomo

Depois de instalar o serviço de função serviço de Federação e configurar os certificados necessários em um computador, você está pronto para configurar o computador para se tornar um servidor de Federação. Você pode usar o procedimento a seguir para configurar o computador para se tornar um suporte\-servidor de Federação autônomo. O ato de criar um suporte\-servidor de Federação autônomo também cria um novo serviço de Federação. Criar um servidor de federação com o Assistente de configuração do servidor de Federação do AD FS.  
  
> [!NOTE]  
> Para um único de Web federado\-sinal\-na \(SSO\) design, você deve ter pelo menos um servidor de federação na organização do parceiro de conta e pelo menos um servidor de federação na organização do parceiro de recurso . Para obter mais informações, consulte [Onde colocar um servidor de federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Para criar um suporte\-servidor de Federação autônomo  
  
1.  Há duas maneiras de iniciar o Assistente de configuração do servidor de Federação do AD FS. Para iniciar o assistente, tome uma das seguintes ações:  
  
    -   Após a instalação do serviço de função serviço de Federação for concluída, abra o snap de gerenciamento do AD FS\-em e clique em de **Assistente de configuração do servidor de Federação do AD FS** no link a **visão geral** página ou o **ações** painel.  
  
    -   A qualquer momento depois que o Assistente de instalação for concluída, abra o Windows Explorer, navegue até a **c:\\Windows\\ADFS** pasta e, em seguida, double\-clique **FsConfigWizard.exe**.  
  
2.  Na página **Bem-vindo**, verifique se **Criar um novo Serviço de Federação** está selecionado e clique em **Avançar**.  
  
3.  No **selecionar espera\-Alone ou implantação de Farm** , clique em **espera\-servidor de Federação autônomo**e, em seguida, clique em **Avançar**.  
  
    > [!IMPORTANT]  
    > Quando você seleciona o suporte do\-opção de servidor de Federação autônomo no Assistente de configuração do servidor de Federação do AD FS, a conta de serviço associada a esse serviço de Federação será automaticamente atribuída à conta de serviço de rede. Usando o serviço de rede como a conta de serviço é recomendado somente em situações em que você estiver avaliando o AD FS em um ambiente de laboratório de teste. Se você pretende usar o suporte do\-opção de servidor de Federação autônomo para implantar um servidor de Federação em um ambiente de produção, é importante que você alterar essa conta de serviço para uma conta de serviço mais adequada que pode ser dedicada a serviços solicitações para esse novo serviço de Federação. Alterando a conta de serviço para uma conta diferente do serviço de rede será mitigar os vetores de ataque, caso contrário, tornaria o servidor de Federação vulnerável a ataques mal-intencionados.  
  
4.  Na página **Especificar o Nome do Serviço de Federação**, verifique se o **Certificado SSL** que aparece está correto. Se não estiver, selecione o certificado apropriado na **certificado SSL** lista.  
  
    Esse certificado é gerado a partir do Secure Sockets Layer \(SSL\) configurações para o Site da Web padrão. Se o Site Padrão tiver somente um certificado SSL configurado, esse certificado será apresentado e automaticamente selecionado para uso. Se vários certificados SSL forem configurados para o Site Padrão, todos esses certificados serão listados aqui e você deverá selecionar um dentre eles. Se não houver configurações do SSL definidas para o Site Padrão, a lista será gerada dos certificados disponíveis no repositório de certificados pessoal no computador local.  
  
    > [!NOTE]  
    > O assistente não permitirá que você substitua o certificado se um certificado SSL estiver configurado para IIS. Isso assegura que qualquer configuração anterior do IIS pretendida para certificados SSL seja preservada. Para contornar essa restrição, você pode remover o certificado ou reconfigurar manualmente-lo com o Console de gerenciamento do IIS.  
  
5.  Se o banco de dados do AD FS que você selecionou já existir, o **banco de dados existente do AD FS Configuration detectado** página será exibida. Se isso ocorrer, clique em **Excluir banco de dados** e clique em **Avançar**.  
  
    > [!CAUTION]  
    > Selecione esta opção apenas quando tiver certeza de que os dados neste banco de dados do AD FS não são importantes ou que não é usado em um farm de servidores de federação de produção.  
  
6.  Na página **Pronto para Aplicar Configurações**, examine os detalhes. Se as configurações parecerem corretas, clique em **próxima** para começar a configurar o AD FS com essas configurações.  
  
7.  Na página **Resultados da Configuração**, examine os resultados. Quando todas as etapas de configuração estiverem concluídas, clique em **fechar** para sair do assistente.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  

