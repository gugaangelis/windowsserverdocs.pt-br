---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: Criar um servidor de federação autônomo
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 112688974c7512923f14578d1617f38266229f43
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966008"
---
# <a name="create-a-stand-alone-federation-server"></a>Criar um servidor de federação autônomo

Depois de instalar o Serviço de Federação serviço de função e configurar os certificados necessários em um computador, você estará pronto para configurar o computador para se tornar um servidor de Federação. Você pode usar o procedimento a seguir para configurar o computador para se tornar um \- servidor de Federação autônomo. O ato de criar um \- servidor de Federação autônomo também cria um novo serviço de Federação. Você cria um servidor de Federação com o assistente de configuração do servidor de Federação AD FS.  
  
> [!NOTE]  
> Para o design de SSO de logon único da Web federada \- \- \( \) , você deve ter pelo menos um servidor de Federação na organização do parceiro de conta e pelo menos um servidor de Federação na organização do parceiro de recurso. Para obter mais informações, consulte [Onde colocar um servidor de federação](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dd807127(v=ws.11)).  
  
A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examinar os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) \( http: \/ \/ go.Microsoft.com \/ fwlink \/ ? LinkId \= 83477 \) .   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Para criar um \- servidor de Federação autônomo  
  
1.  Ha duas maneiras para iniciar o Assistente de Configuração do Servidor de Federação AD FS. Para iniciar o assistente, tome uma das seguintes ações:  
  
    -   Após a conclusão da instalação do serviço de função Serviço de Federação, abra o snap-in Gerenciamento de AD FS \- e clique no link **AD FS assistente de configuração do servidor de Federação** na página **visão geral** ou no painel **ações** .  
  
    -   A qualquer momento depois que o assistente de instalação for concluído, abra o Windows Explorer, navegue até a pasta **C: \\ Windows \\ ADFS** e clique duas vezes \- em **FsConfigWizard.exe**.  
  
2.  Na página **Bem-vindo**, verifique que o **Cria um novo Serviço de Federação** esteja selecionado, e então clique em **Próximo**.  
  
3.  Na página **selecionar \- implantação autônoma ou de farm** , clique em **servidor de \- Federação autônomo**e, em seguida, clique em **Avançar**.  
  
    > [!IMPORTANT]  
    > Quando você seleciona a \- opção de servidor de federação autônoma no assistente de configuração do servidor de federação AD FS, a conta de serviço associada a esse serviço de Federação será automaticamente atribuída à conta de serviço de rede. O uso do serviço de rede como a conta de serviço só é recomendado em situações em que você esteja avaliando AD FS em um ambiente de laboratório de teste. Se você pretende usar a opção de \- servidor de Federação autônomo para implantar um servidor de Federação em um ambiente de produção, é importante que você altere essa conta de serviço para uma conta de serviço mais apropriada que possa ser dedicada a atender solicitações para essa nova serviço de Federação. Alterar a conta de serviço para uma conta diferente do serviço de rede reduzirá possíveis vetores de ataque que, de outra forma, tornaria o servidor de Federação vulnerável a ataques mal-intencionados.  
  
4.  Na página **Especificar o Nome do Serviço de Federação**, verifique que o **Certificado SSL** que é exibido seja o correto. Caso contrário, selecione o certificado apropriado na lista **certificado SSL** .  
  
    Esse certificado é gerado a partir das \( configurações de protocolo SSL SSL \) para o site padrão. Se o Site Padrão tiver somente um certificado SSL configurado, esse certificado será apresentado e automaticamente selecionado para uso. Se vários certificados SSL forem configurados para o Site Padrão, todos esses certificados serão listados aqui e você deverá selecionar um dentre eles. Se não houver configurações do SSL definidas para o Site Padrão, a lista será gerada dos certificados disponíveis no repositório de certificados pessoal no computador local.  
  
    > [!NOTE]  
    > O assistente não permitirá que você substitua o certificado se um certificado SSL estiver configurado para IIS. Isso assegura que qualquer configuração anterior do IIS pretendida para certificados SSL seja preservada. Para contornar essa restrição, você pode remover o certificado ou reconfigurá-lo manualmente com o console de gerenciamento do IIS.  
  
5.  Se o banco de dados do AD FS que você selecionou já existir, a página **Banco de Dados de Configuração do AD FS Existente Detectada** aparece. Se isso ocorrer, clique em **Excluir banco de dados**, e depois clique em **Próximo**.  
  
    > [!CAUTION]  
    > Selecione esta opção apenas quando você tiver certeza que os dados neste banco de dados do AD FS não forem importantes ou que não é usado em um farm de servidor de federação.  
  
6.  Na página **Pronto para Aplicar as Configurações**, verificar os detalhes. Se as configurações parecem estar certas, clique em **Próximo** para começar configurar o AD FS com estas configurações.  
  
7.  Na página **Resultados de Configuração**, analise os resultados. Quando todas as etapas de configuração forem concluídas, clique em **fechar** para sair do assistente.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Como configurar um servidor de federação](Checklist--Setting-Up-a-Federation-Server.md)  
  
