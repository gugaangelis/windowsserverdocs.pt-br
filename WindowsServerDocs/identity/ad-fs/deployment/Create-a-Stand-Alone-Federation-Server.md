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
ms.openlocfilehash: 603786d8553cce20f0b559ba8a91dfc29f760488
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855479"
---
# <a name="create-a-stand-alone-federation-server"></a>Criar um servidor de federação autônomo

Depois de instalar o Serviço de Federação serviço de função e configurar os certificados necessários em um computador, você estará pronto para configurar o computador para se tornar um servidor de Federação. Você pode usar o procedimento a seguir para configurar o computador para se tornar um servidor de Federação autônomo\-. O ato de criar um servidor de Federação autônomo\-também cria um novo Serviço de Federação. Você cria um servidor de Federação com o assistente de configuração do servidor de Federação AD FS.  
  
> [!NOTE]  
> Para o\-de logon\-único da Web federado em \(design de\) SSO, você deve ter pelo menos um servidor de Federação na organização do parceiro de conta e pelo menos um servidor de Federação na organização do parceiro de recurso. Para obter mais informações, consulte [Onde colocar um servidor de federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=\)83477.   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Para criar um servidor de Federação autônomo\-  
  
1.  Ha duas maneiras para iniciar o Assistente de Configuração do Servidor de Federação AD FS. Para iniciar o assistente,  faça o seguinte:  
  
    -   Depois que a instalação do serviço de função Serviço de Federação for concluída, abra o\-snap de gerenciamento de AD FS no e clique no link **Assistente de configuração do servidor de federação AD FS** na página **visão geral** ou no painel **ações** .  
  
    -   A qualquer momento após o assistente de instalação ser concluído, abra o Windows Explorer, navegue até a pasta **C:\\Windows\\ADFS** e clique duas vezes\-clique em **FsConfigWizard. exe**.  
  
2.  Na página **Bem-vindo**, verifique que o **Cria um novo Serviço de Federação** esteja selecionado, e então clique em **Próximo**.  
  
3.  Na página **selecionar\-autônomo ou implantação de farm** , clique em **\-servidor de Federação**autônomo e, em seguida, clique em **Avançar**.  
  
    > [!IMPORTANT]  
    > Quando você seleciona a opção de servidor de Federação\-autônomo no assistente de configuração do servidor de Federação AD FS, a conta de serviço associada a esse Serviço de Federação será automaticamente atribuída à conta de serviço de rede. O uso do serviço de rede como a conta de serviço só é recomendado em situações em que você esteja avaliando AD FS em um ambiente de laboratório de teste. Se você pretende usar a opção de servidor de federação autônoma\-para implantar um servidor de Federação em um ambiente de produção, é importante que você altere essa conta de serviço para uma conta de serviço mais apropriada que possa ser dedicada a atender solicitações para essa nova Serviço de Federação. Alterar a conta de serviço para uma conta diferente do serviço de rede reduzirá possíveis vetores de ataque que, de outra forma, tornaria o servidor de Federação vulnerável a ataques mal-intencionados.  
  
4.  Na página **Especificar o Nome do Serviço de Federação**, verifique que o **Certificado SSL** que é exibido seja o correto. Caso contrário, selecione o certificado apropriado na lista **certificado SSL** .  
  
    Esse certificado é gerado a partir do protocolo SSL \(configurações de\) SSL para o site padrão. Se o Site Padrão só tiver um certificado SSL configurado, aquele certificado é apresentado e automaticamente selecionado para uso. Se vários certificados SSL são configurados para o Site Padrão, todos aqueles certificados são listados aqui e você deve selecionar de entre eles. Se não houver configurações SSL definidas para o Site Padrão, a lista é gerada a partir dos certificados que estiverem disponíveis no armazenamento pessoal de certificados no computador local.  
  
    > [!NOTE]  
    > O assistente não permitirá a substituição do certificado se um certificado SSL for configurado para o IIS. Isso assegura que qualquer objetivo antes da configuração do IIS para certificados SSL seja preservado. Para contornar essa restrição, você pode remover o certificado ou reconfigurá-lo manualmente com o console de gerenciamento do IIS.  
  
5.  Se o banco de dados do AD FS que você selecionou já existir, a página **Banco de Dados de Configuração do AD FS Existente Detectada** aparece. Se isso ocorrer, clique em **Excluir banco de dados**, e depois clique em **Próximo**.  
  
    > [!CAUTION]  
    > Selecione esta opção apenas quando você tiver certeza que os dados neste banco de dados do AD FS não forem importantes ou que não é usado em um farm de servidor de federação.  
  
6.  Na página **Pronto para Aplicar as Configurações**, verificar os detalhes. Se as configurações parecem estar certas, clique em **Próximo** para começar configurar o AD FS com estas configurações.  
  
7.  Na página **Resultados de Configuração**, analise os resultados. Quando todas as etapas de configuração forem concluídas, clique em **fechar** para sair do assistente.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  

