---
title: Gerenciar o Server Core
description: Saiba como gerenciar uma instalação Server Core do Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 07/23/2019
ms.openlocfilehash: bbb04e761dbb1dd48d95e15d11c91608f4d6c240
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476544"
---
# <a name="manage-a-server-core-server"></a>Gerenciar um servidor Server Core
 
> Aplica-se a: Windows Server 2019, Windows Server 2016 e Windows Server (canal semestral)

Você pode gerenciar um servidor Server Core das seguintes maneiras:
- Usando o [centro de administração do Windows](../../manage/windows-admin-center/overview.md)
- Usando [ferramentas de administração de servidor remoto](../../remote/remote-server-administration-tools.md) em execução no Windows 10
- Local e remotamente usando o Windows PowerShell
- Usando [Gerenciador do servidor](../server-manager/server-manager.md) remotamente
- Usando remotamente um [snap-in do MMC](#managing-with-microsoft-management-console)
- Remotamente com [serviços de área de trabalho remota](#managing-with-remote-desktop-services)

Você também pode adicionar hardware e gerenciar drivers localmente, desde que faça isso na linha de comando.

Há algumas limitações e dicas importantes para ter em mente quando você trabalha com o Server Core:

- Se você fechar todas as janelas de prompt de comando e quiser abrir uma nova janela de prompt de comando, poderá fazer isso no Gerenciador de tarefas. Pressione **Ctrl\+ALT\+Delete**, clique em **iniciar Gerenciador de tarefas**, clique em **mais detalhes > Arquivo > executar**e digite **cmd. exe**. (Digite **PowerShell. exe** para abrir uma janela de comando do PowerShell.) Como alternativa, você pode sair e entrar novamente.
- Nenhum comando ou ferramenta que tente iniciar o Windows Explorer vai funcionar. Por exemplo, executar **Iniciar.** de um prompt de comando não funcionará.
- Não há suporte para renderização HTML ou ajuda HTML no Server Core.
- O Server Core dá suporte a Windows Installer no modo silencioso para que você possa instalar ferramentas e utilitários de Windows Installer arquivos. Ao instalar Windows Installer pacotes no Server Core, use a opção **/QB** para exibir a interface do usuário básica.
- Para alterar o fuso horário, execute **Set-Date**.
- Para alterar as configurações internacionais, execute **Control intl. cpl**.
- O **Control. exe** não será executado por conta própria. Você deve executá-lo com **timedate. cpl** ou **intl. cpl**.
- **Winver. exe** não está disponível no Server Core. Para obter informações de versão, use **SystemInfo. exe**.

## <a name="managing-server-core-with-windows-admin-center"></a>Gerenciando o Server Core com o centro de administração do Windows
O [Windows Admin Center](../../manage/windows-admin-center/overview.md) é um aplicativo de gerenciamento baseado em navegador que permite a administração no local de servidores do Windows com nenhuma dependência do Azure ou nuvem. O Windows Admin Center oferece controle total sobre todos os aspectos da sua infraestrutura de servidor e é útil para o gerenciamento de redes privadas que não estão conectadas à Internet. Você pode instalar o centro de administração do Windows no Windows 10, em um servidor de gateway ou em uma instalação do Windows Server com a experiência desktop e, em seguida, conectar-se ao sistema Server Core que você deseja gerenciar.

## <a name="managing-server-core-remotely-with-server-manager"></a>Gerenciando o Server Core remotamente com o Gerenciador do Servidor

Gerenciador do Servidor é um console de gerenciamento do Windows Server que ajuda você a provisionar e gerenciar servidores locais e remotos baseados no Windows a partir de suas áreas de trabalho, sem a necessidade de acesso físico aos servidores ou da necessidade de habilitar o protocolo de Área de Trabalho Remota (RDP) conexões com cada servidor. O Gerenciador do Servidor dá suporte ao gerenciamento remoto de vários servidores.

Para permitir que o servidor local seja gerenciado pelo Gerenciador do Servidor em execução em um servidor remoto, execute o cmdlet do Windows PowerShell **Configure-SMRemoting. exe – Enable**.

## <a name="managing-with-microsoft-management-console"></a>Gerenciando com o console de gerenciamento Microsoft

Você pode usar muitos snap-ins para o MMC (console de gerenciamento Microsoft) remotamente para gerenciar o servidor Server Core.

Para usar um snap-in do MMC para gerenciar um servidor Server Core que seja membro do domínio: 

1. Inicie um snap-in do MMC, como o gerenciamento do computador.
2. Clique com o botão direito do mouse no snap-in e clique em **conectar-se a outro computador**.
2. Digite o nome do computador do servidor Server Core e clique em **OK**. Agora você pode usar o snap-in do MMC para gerenciar o servidor Server Core como faria com qualquer outro PC ou servidor.

Para usar um snap-in do MMC para gerenciar um servidor Server Core que *não* seja um membro do domínio: 

1. Estabeleça credenciais alternativas para usar para se conectar ao computador Server Core digitando o seguinte comando em um prompt de comando no computador remoto:

   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```

   Se você quiser ser solicitado a fornecer uma senha, omita a opção **/Pass** .

2. Quando solicitado, digite a senha para o nome de usuário especificado.
   Se o firewall no servidor Server Core ainda não estiver configurado para permitir que os snap-ins do MMC se conectem, siga as etapas abaixo para configurar o Firewall do Windows para permitir o snap-in do MMC. Em seguida, continue com a etapa 3.
3. Em um computador diferente, inicie um snap-in do MMC, como o **Gerenciamento do computador**.
4. No painel esquerdo, clique com o botão direito do mouse no snap-in e clique em **conectar a outro computador**. (Por exemplo, no exemplo de gerenciamento do computador, clique com o botão direito do mouse em **Gerenciamento do computador (local)** .)
5. Em **outro computador**, digite o nome do computador do servidor Server Core e clique em **OK**. Agora, você pode usar o snap-in do MMC para gerenciar o servidor do Server Core, como faria com qualquer outro computador executando um sistema operacional Windows Server.

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>Para configurar o Firewall do Windows para permitir a conexão do(s) snap-in(s) do MMC
Para permitir que todos os snap-ins do MMC se conectem, execute o seguinte comando:

```PowerShell
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

Para permitir que apenas snap-ins específicos do MMC se conectem, execute o seguinte:

```PowerShell
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

Em  que o Ruler é um dos seguintes, dependendo de qual snap-in você deseja conectar:

| Snap-in do MMC                            | Grupo de regras                                            |
| ---------------------------------------- | ------------------------------------------------------- |
| Visualizador de Eventos                           | Gerenciamento Remoto do Log de Eventos                           |
| Serviços                               | Gerenciamento Remoto de Serviços                             |
| Pastas compartilhadas                         | Compartilhamento de Arquivo e Impressora                              |
| Agendador de Tarefas                         | Logs e Alertas de Desempenho, compartilhamento de arquivos e impressoras |
| Gerenciamento de disco                        | Gerenciamento de Volumes Remoto                              |
| Firewall do Windows e segurança avançada | Gerenciamento Remoto do Firewall do Windows                    |


> [!NOTE] 
> Alguns snap-ins do MMC não têm um grupo de regras correspondente que permite que eles se conectem por meio do firewall. No entanto, a habilitação dos grupos de regras para Visualizador de Eventos, Serviços ou Pastas Compartilhadas permitirá que a maioria dos outros snap-ins se conectem. 
>
> Além disso, determinados snap-ins exigem configuração adicional para se conectarem através do Firewall do Windows:
>
> - Gerenciamento de Disco. Primeiro, você deve iniciar o VDS (Serviço de Disco Virtual) no computador Server Core. Você também deve configurar as regras de Gerenciamento de Disco adequadamente no computador que está executando o snap-in do MMC.
> - Monitor de segurança IP. Primeiro, você deve habilitar o gerenciamento remoto deste snap-in. Para fazer isso, em um prompt de comando, digite **cscript \windows\system32\scregedit.wsf/im 1**
> - Confiabilidade e Desempenho. O snap-in não requer nenhuma configuração adicional, mas, quando você usá-lo para monitorar um computador Server Core, só poderá monitorar os dados de desempenho. Dados de confiabilidade não estão disponíveis.

## <a name="managing-with-remote-desktop-services"></a>Gerenciando com o Serviços de Área de Trabalho Remota

Você pode usar [área de trabalho remota](../../remote/remote-desktop-services/welcome-to-rds.md) para gerenciar um servidor Server Core de computadores remotos.

Antes de poder acessar o Server Core, você precisará executar o seguinte comando: 

```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```

Isso permite que a Área de Trabalho Remota do modo Administração aceite conexões.

## <a name="add-hardware-and-manage-drivers-locally"></a>Adicionar hardware e gerenciar drivers localmente

Para adicionar hardware a um servidor Server Core, siga as instruções fornecidas pelo fornecedor de hardware para instalar o novo hardware. 

Se o hardware não for plug and Play, você precisará instalar o driver manualmente. Para fazer isso, copie os arquivos de driver para um local temporário no servidor e, em seguida, execute o seguinte comando:

```
pnputil –i –a <driverinf>
```

Em que *driverinf* é o nome de arquivo do arquivo. inf para o driver.

Se solicitado, reinicie o computador.

Para ver quais drivers estão instalados, execute o seguinte comando: 

```
sc query type= driver
```

> [!NOTE] 
> Você deve incluir o espaço após o sinal de igual para o comando ser concluído com êxito.

Para desabilitar um driver de dispositivo, execute o seguinte:

```
sc delete <service_name>
```

Em que *nome_do_serviço* é o nome do serviço que você obteve ao executar a **consulta SC Type = driver**.