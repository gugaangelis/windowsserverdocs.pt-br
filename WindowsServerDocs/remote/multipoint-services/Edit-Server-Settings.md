---
title: Editar configurações do servidor
description: Saiba mais sobre configurações do MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afb64b94-9055-4703-b8ce-a8839b2718da
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 105b10428835d11a0ea0661fe2fa7d57f80a1aba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885177"
---
# <a name="edit-server-settings"></a>Editar configurações do servidor
Quando você instalou o MultiPoint Services, você configurou as configurações do seu sistema, incluindo optar por participar de determinados programas. Este tópico descreve as configurações que você pode definir para o seu sistema MultiPoint Services e explica como editar as configurações.  
  
## <a name="about-multipoint-services-settings"></a>Sobre configurações do MultiPoint Services  
A tabela a seguir descreve as diferentes configurações que você pode alterar para o seu sistema MultiPoint Services.  
  
|Configuração do MultiPoint Services|Descrição|  
|-----------------------------------------------------------------------------------------|---------------|  
|Permitir que uma conta tenha várias sessões|Permite que uma única conta de usuário esteja conectada simultaneamente a várias estações. Isso pode ser útil em casos como uma sala de aula onde cada aluno está usando uma conta única e compartilhada. Usando essa configuração, quaisquer alterações nos recursos da conta, como pastas de documentos ou a área de trabalho, estão disponíveis para todos os usuários conectados usando a mesma conta.|  
|Permitir que este computador seja gerenciado remotamente|Permite que o computador que está executando o MultiPoint Services seja gerenciado por outros sistemas MultiPoint na sua rede. Se essa opção estiver selecionada e o computador de gerenciamento estiver na mesma sub-rede, esse computador aparecerá na lista de servidores disponíveis para serem gerenciados. Se essa opção estiver selecionada e o computador de gerenciamento estiver em uma sub-rede diferente, o computador de gerenciamento ainda poderá gerenciar esse computador, mas você deve especificar o endereço IP do computador.|
|Permitir o monitoramento das áreas de trabalho deste computador|Permite que você controle se as áreas de trabalho podem ser monitoradas no sistema MultiPoint Services. Se essa configuração estiver desativada (não selecionadas), áreas de trabalho de estações (locais e remotas) que estão conectadas ao computador que está executando o MultiPoint Services não exibirá na guia página inicial do MultiPoint Manager (incluindo em um computador diferente se o computador estiver sendo gerenciados remotamente).|  
|Sempre iniciar no modo de console|Habilita a tecnologia RemoteFX, que foi projetada para possibilitar sessões da Área de Trabalho Remota mais rápidas e eficientes ao descarregar o processamento na CPU e GPU. Se você estiver se conectando ao MultiPoint Services usando um cliente compatíveis com RemoteFX, você poderá obter melhor desempenho usando essa opção. Os benefícios dependem dos recursos do seu servidor e rede. Por exemplo, isso depende, em partes, se o tempo gasto fazendo processamento adicional para compactar o fluxo de dados é menor do que o tempo poupado pela transmissão de menos dados.|  
|Não mostrar notificação de privacidade no primeiro logon do usuário|Quando um usuário faz logon em uma estação do MultiPoint pela primeira vez, uma notificação é exibida para avisar o usuário de que as atividades da estação dele podem ser monitoradas.|  
|Atribuir um IP exclusivo a cada estação|Atribui um endereço IP exclusivo a cada estação. Por padrão, o MultiPoint Services tem um endereço IP, que é compartilhado com todas as sessões que estão em execução no sistema. Essa configuração, no entanto, pode causar alguns problemas de compatibilidade do aplicativo. Por exemplo, se um aplicativo requer um endereço IP exclusivo, ele pode não ser executado corretamente o MultiPoint Services. Selecionar essa opção, também conhecido como virtualização de IP, pode resolver esse problema.<br /><br />A virtualização de IP também é útil para monitorar sessões ativas no MultiPoint Services. Algumas ferramentas de monitoramento reportam a utilização de acordo com o endereço IP. Para habilitar o monitoramento de sessão, você pode usar a virtualização de IP para atribuir um endereço IP exclusivo a cada sessão. Observe que, se você selecionar essa opção, cada nova sessão receberá um endereço IP exclusivo. Todas as sessões existentes continuam usando o endereço IP compartilhado até que elas sejam desconectadas e conectadas novamente.|  
|Permitir mensagens Instantâneas entre o MultiPoint Dashboard e uma sessão de usuário neste computador|Habilita o chat entre o MultiPoint Manager e a sessão do usuário nesse computador. Para obter mais informações, consulte [Usar IM](Use-IM.md).|  
|Permitir a orquestração de sessões do usuário do MultiPoint Dashboard e do administrador|Quando habilitada, permite que os administradores usem o painel do MultiPoint Dashboard para orquestrar sessões. Essas sessões são exibidas como miniaturas.|  
|Permite que as estações usem a renderização de hardware da GPU|Controla se as estações podem usar a unidade de processamento gráfico do sistema (GPU).|   
  
## <a name="editing-the-computer-settings"></a>Editar as configurações do computador  
  
1.  Abra o MultiPoint Manager no [modo de estação](Switch-Between-Modes.md)e, em seguida, clique no **Home** guia.  
  
2.  No **computador** coluna, clique no nome do computador e, nas *nome do computador* **tarefas**, clique em **editar configurações do computador**.  
  
3.  Marque ou desmarque os itens que você deseja alterar e, em seguida, clique em **Okey**.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciar tarefas de sistema usando o MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md)  
  
