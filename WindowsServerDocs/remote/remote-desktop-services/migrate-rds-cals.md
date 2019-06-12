---
title: Migrar suas Licenças de Acesso para Cliente de Serviços de Área de Trabalho Remota (RDS CALs)
description: Este artigo descreve como migrar suas licenças de acesso do cliente de serviços da área de trabalho remota para novos servidores de licença do Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
msreviewer: ''
nams.suite: ''
nams.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 91bdedce-6145-469f-b72e-7e113c4391e9
author: christianmontoya
manager: scottman
ms.openlocfilehash: c947375b58c0ad88781335b799055e101bd2a193
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447102"
---
# <a name="migrate-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Migrar suas Licenças de Acesso para Cliente de Serviços de Área de Trabalho Remota (RDS CALs)

Você tem três opções para migrar as RDS CALs:
1. Método de conexão automática: Este método recomendado se comunica por meio da internet diretamente para a Microsoft Clearinghouse saída pela porta TCP 443.  
2. Usando um navegador da web: Esse método permite a migração quando o servidor que executa a ferramenta Gerenciador de licenciamento de área de trabalho remota não tem conectividade com a internet, mas o administrador tem conectividade com a internet em um dispositivo separado. A URL para o método de migração da Web é exibida no Assistente de gerenciamento de RDS CALs. 
3. Usando um telefone: Esse método permite ao administrador concluir o processo de migração por telefone com um representante da Microsoft. O número de telefone apropriado é determinado por país/região que você escolheu no Assistente para ativação do servidor e é exibido no Assistente de gerenciamento de RDS CALs.

Neste artigo, o [método de migração de estabelecer o RDS CAL](#establish-rds-cal-migration-method) destaca as etapas gerais comuns em qualquer método de migração de RDS CAL, enquanto [migrar RDS CALs](#migrate-rds-cals) destaca as etapas específicas para cada migração método.

Independentemente do método de migração, você deve, no mínimo, ser um membro do grupo Administradores local para executar as etapas de migração.

## <a name="establish-rds-cal-migration-method"></a>Estabelecer o método de migração de RDS CAL

1. No servidor de licença, abra **o Gerenciador de licenciamento de área de trabalho remota**. (Clique em **Iniciar > Ferramentas administrativas**. Insira o **dos serviços de área de trabalho remota** diretório e inicie **Gerenciador de licenciamento de área de trabalho remota**.)
2. Verifique se o método de conexão para o servidor de licenças de área de trabalho remota: o servidor de licenças ao qual você deseja migrar as RDS CALs e, em seguida, clique com o botão direito **propriedades**. No **método de Conexão** guia, verifique se o **método de Conexão** -você pode alterá-lo no menu suspenso. Clique em **OK**.
3. O servidor de licenças ao qual você deseja migrar as RDS CALs e, em seguida, clique com o botão direito **Gerenciar RDS CALs**.
4. Siga as etapas no Assistente para o **seleção de ação** página. Clique em **migrar do RDS CALs de outro servidor de licença para este servidor de licença**.
6. Escolha o motivo para migrar as RDS CALs e, em seguida, clique em **próxima**. Você tem as seguintes opções:
    - O servidor de licença de origem está sendo substituído por este servidor de licença.
    - O servidor de licença de origem não está mais funcionando.
7. A próxima página do assistente depende do motivo de migração que você escolheu.
    - Se você escolheu **o servidor de licença de origem está sendo substituído por este servidor de licença** como o motivo para migrar as RDS CALs, o **informações do servidor de licença de origem** página é exibida.
    
       Na página de informações do servidor de licença de origem, insira o nome ou endereço IP do servidor de licença de origem.

       Se o servidor de licença de origem está disponível na rede, clique em **próxima**. O assistente entra em contato com o servidor de licença de origem. Se o servidor de licença de origem estiver executando um sistema operacional anterior ao Windows Server 2008 R2 ou o servidor de licença de origem é desativado, será exibido um lembrete que você deve remover as RDS CALs manualmente do servidor de licença de origem após a conclusão do assistente. Depois de confirmar que você entenda a esse requisito, o **obter pacote de chaves de licença de cliente** página será exibida.

       Se o servidor de licença de origem não está disponível na rede, selecione **o servidor de licença de origem especificado não está disponível na rede**. Especifique o sistema operacional que está executando o servidor de licença de origem e, em seguida, forneça a ID do servidor de licença para o servidor de licença de origem. Depois de clicar em **próxima**, será exibido um lembrete que você deve remover as RDS CALs manualmente do servidor de licença de origem após a conclusão do assistente. Depois de confirmar que você entenda a esse requisito, o **obter pacote de chaves de licença de cliente** página será exibida.

    - Se você escolheu **o servidor de licença de origem não está mais funcionando** como o motivo para migrar as RDS CALs, será exibido um lembrete que você deve remover as RDS CALs manualmente do servidor de licença de origem após a conclusão do assistente. Depois de confirmar que você entenda a esse requisito, o **obter pacote de chaves de licença de cliente** página será exibida.

A próxima etapa é migrar as CALs - use as informações abaixo para concluir o assistente. Observe que o que você vê no assistente depende do método de conexão que você identificou na etapa 2 acima.

## <a name="migrate-rds-cals"></a>Migrar RDS CALs

Existem três mecanismos para migrar licenças para o servidor de licença de destino. Continue as etapas correspondentes para o **método de Conexão** verificados na etapa 2:
  - [Método de conexão automática](#automatic-connection-method)
  - [Usando um navegador da web](#using-a-web-browser)
  - [Usando um telefone](#using-a-telephone)

### <a name="automatic-connection-method"></a>Método de conexão automática

1. No **License Program** página, selecione o programa apropriado por meio do qual você adquiriu as RDS CALs e clique em **próxima**.
2. Insira as informações necessárias (normalmente um código de licença ou um número de contrato, dependendo do **License program**) e, em seguida, clique em **próxima**. Consulte a documentação fornecida quando você adquiriu as RDS CALs.
4. Selecione a versão apropriada do produto, o tipo de licença e a quantidade de RDS CALs para seu ambiente com base no seu contrato de compra de RDS CAL e, em seguida, clique em **próxima**.
5. A Microsoft Clearinghouse é contatada automaticamente e processa sua solicitação. As RDS CALs são migradas, em seguida, no servidor de licenças.
6. Clique em **concluir** para concluir o processo de migração de RDS CAL.

### <a name="using-a-web-browser"></a>Usando um navegador da web
1. Sobre o **obter pacote de chaves de licença de cliente** página, clique no hiperlink para se conectar ao site do licenciamento dos serviços de área de trabalho remota.
   Se você estiver executando o Gerenciador de licenciamento de área de trabalho remota em um computador que não tem conectividade com a Internet, anote o endereço do site do licenciamento dos serviços de área de trabalho remota e, em seguida, conecte-se ao site da Web de um computador que tenha conectividade com a Internet. 
2. Na página de Web de licenciamento do serviços de área de trabalho remota, sob **opção de selecionar**, selecione **Gerenciar CALs**e, em seguida, clique em **próxima**.
3. Forneça as seguintes informações obrigatórias, e clique em **próxima**:
    - **ID do servidor de licença de destino**: Um número de dígitos de 35, em grupos de 5 algarismos, que é exibido na **obter pacote de chaves de licença de cliente** página no Assistente de gerenciamento de RDS CALs.
    - **Motivo para a recuperação**: Escolha o motivo para migrar as RDS CALs.
    - **Programa de licença**: Escolha o programa por meio do qual você adquiriu as RDS CALs.
4. Forneça as seguintes informações obrigatórias, e clique em **próxima**:
   - Sobrenome
   - Nome ou o nome fornecido
   - Nome da empresa
   - País/região

     Você também pode fornecer as informações opcionais solicitadas, como endereço da empresa, endereço de email e número de telefone. No campo unidade organizacional, você pode descrever a unidade de sua organização que serve este servidor de licença.

5. O programa de licença que você selecionou na página anterior determina quais informações você precisa fornecer na próxima página. Na maioria dos casos, você deve fornecer um código de licença ou um número de contrato. Consulte a documentação fornecida quando você adquiriu as RDS CALs. Além disso, você precisa especificar o tipo de RDS CAL e a quantidade que você deseja migrar para o servidor de licença.
6. Depois de inserir as informações necessárias, clique em **Avançar**.
7. Verificar se todas as informações que você inseriu está correta e, em seguida, clique em **próxima** para enviar sua solicitação para a Microsoft Clearinghouse. A página da web, em seguida, exibe uma ID de pacote de chaves de licença gerada pela Microsoft Clearinghouse.

   > [!IMPORTANT] 
   > Mantenha uma cópia da ID do pacote de chaves da licença. Ter essas informações com você facilita a comunicação com a Microsoft Clearinghouse, devem precisar de ajuda para recuperar as RDS CALs.

8. No mesmo **obter pacote de chaves de licença do cliente** página, insira a ID do pacote de chaves de licença e, em seguida, clique em **próxima** para migrar as RDS CALs para o servidor de licenças.
9. Clique em **concluir** para concluir o processo de migração de RDS CAL.

### <a name="using-a-telephone"></a>Usando um telefone
1. Sobre o **obter pacote de chaves de licença de cliente** página, use o número de telefone exibido para chamar a Microsoft Clearinghouse. Forneça ao representante sua ID de servidor de licença de área de trabalho remota e as informações necessárias para o programa de licenciamento por meio do qual você adquiriu as RDS CALs. O representante processará sua solicitação para migrar as RDS CALs e lhe dá uma ID exclusiva para as RDS CALs. Essa identificação exclusiva é conhecida como o **ID do pacote de chaves de licença**.

   > [!IMPORTANT]
   > Mantenha uma cópia da ID do pacote de chaves da licença. Ter essas informações com você facilita a comunicação com a Microsoft Clearinghouse devem precisar de ajuda para recuperar as RDS CALs.

2. No mesmo **obter pacote de chaves de licença do cliente** página, insira a ID do pacote de chaves de licença e, em seguida, clique em **próxima** para migrar as RDS CALs para o servidor de licenças.
3. Clique em **concluir** para concluir o processo de migração de RDS CAL.
