---
ms.assetid: ab97948a-c434-48f2-8313-c1a7a518e5f7
title: "Criar um servidor de Federação autônomo"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fd075c5b7d1bfce89cc27c4917a016e7e5037ce5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-stand-alone-federation-server"></a>Criar um servidor de Federação autônomo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de instalar o serviço de função do serviço de Federação e configurar os certificados necessários em um computador, você estará pronto para configurar o computador para se tornar um servidor de Federação. Você pode usar o procedimento a seguir para configurar o computador para se tornar um servidor de Federação stand\ sozinho. O ato de criação de um servidor de Federação stand\ sozinho também cria um novo serviço de Federação. Você cria um servidor de federação com o Assistente para configuração do servidor de Federação do AD FS.  
  
> [!NOTE]  
> Para o design \(SSO\) federados Single\-Sign\-On da Web, você deve ter pelo menos um servidor de federação na organização do parceiro de conta e pelo menos um servidor de federação na organização do parceiro de recurso. Para obter mais informações, consulte [onde colocar um servidor de Federação](https://technet.microsoft.com/library/dd807127.aspx).  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
### <a name="to-create-a-stand-alone-federation-server"></a>Para criar um servidor de Federação stand\ sozinho  
  
1.  Há duas maneiras de iniciar o Assistente para configuração do servidor de Federação do AD FS. Para iniciá-lo, siga um destes procedimentos:  
  
    -   Após a instalação de serviço de função do serviço de Federação for concluída, abra o AD FS snap\-in Gerenciamento e clique no **Assistente de configuração de servidor de Federação do AD FS** vincular no **visão geral** página ou no **ações** painel.  
  
    -   A qualquer momento depois que o Assistente para instalação for concluída, abra o Windows Explorer, navegue até o **C:\\Windows\\ADFS** pasta e, em seguida, clique em double\ **FsConfigWizard.exe**.  
  
2.  No **boas-vindas** página, verifique **criar um novo serviço de Federação** está selecionado e clique em **próxima**.  
  
3.  No **selecione Stand\-sozinho ou implantação Farm** página, clique em **Stand\ sozinho servidor de Federação**e clique em **próxima**.  
  
    > [!IMPORTANT]  
    > Quando você selecionar a opção de servidor de Federação Stand\ sozinho no Assistente para configuração do servidor de Federação do AD FS, a conta de serviço associada com esse serviço de Federação será atribuída automaticamente para a conta de serviço de rede. Usando o serviço de rede como a conta de serviço só é recomendado em situações onde você está avaliando AD FS em um ambiente de laboratório de teste. Se você pretende usar a opção de servidor de Federação Stand\ sozinho para implantar um servidor de Federação em um ambiente de produção, é importante que você alterar essa conta de serviço para uma conta de serviço mais apropriada que pode ser dedicada a atender às solicitações para esse novo serviço de Federação. Alterar a conta de serviço para uma conta diferente de serviço de rede reduz possíveis vetores de ataque que caso contrário, o servidor de Federação vulnerável a ataques mal-intencionados.  
  
4.  No **especificar o nome do serviço de Federação** página, verifique o **certificado SSL** que está mostrando está correta. Se não, selecione o certificado apropriado do **certificado SSL** lista.  
  
    Esse certificado é gerado das configurações de Secure Sockets Layer \(SSL\) do site padrão. Se o site padrão tem apenas um certificado SSL configurado, esse certificado é apresentado e selecionado automaticamente para uso. Se vários certificados SSL são configurados para o site padrão, todos esses certificados são listados aqui e você deve selecionar dentre elas. Se não houver nenhuma configuração de SSL para o site padrão, a lista é gerada a partir dos certificados que estão disponíveis no repositório de certificados pessoais no computador local.  
  
    > [!NOTE]  
    > O assistente não permitirá que você substituir o certificado se um certificado SSL está configurado para o IIS. Isso garante que qualquer destinado a configuração anterior do IIS para certificados SSL é preservada. Para contornar essa restrição, você pode remover o certificado ou reconfigurá manualmente-lo com o Console de gerenciamento do IIS.  
  
5.  Se o banco de dados do AD FS que você selecionou já existir, o **existente AD FS configuração do banco de dados detectado** página será exibida. Se isso ocorrer, clique em **banco de dados de excluir**e clique em **próxima**.  
  
    > [!CAUTION]  
    > Selecione essa opção somente quando tiver certeza de que os dados neste banco de dados do AD FS não são importantes ou que não é usado em um farm de servidores de federação de produção.  
  
6.  Sobre o **pronto para aplicar configurações** página, examine os detalhes. Se as configurações parecerem corretas, clique em **próxima** para começar a configurar o AD FS com essas configurações.  
  
7.  Sobre o **resultados de configuração** página, examinar os resultados. Quando terminarem de todas as etapas de configuração, clique em **fechar** para sair do assistente.  
  
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurar um servidor de Federação](Checklist--Setting-Up-a-Federation-Server.md)  
  

