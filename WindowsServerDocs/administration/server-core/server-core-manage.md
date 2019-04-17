---
title: Gerenciar o núcleo do servidor
description: Saiba como gerenciar uma instalação Server Core do Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: 6836e5db36727294d215f7f98e0faeede55a612a
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1718706"
---
# <a name="manage-a-server-core-server"></a>Gerenciar um servidor de núcleo do servidor
 
> Aplica-se a: Windows Server (canal delimitadas anual) e Windows Server 2016

Você pode gerenciar um servidor server Core das seguintes maneiras:
- Usando o [Centro de administração do Windows](../../manage/windows-admin-center/overview.md)
- Usando as [Ferramentas de administração de servidor remoto](../../remote/remote-server-administration-tools.md) em execução no Windows 10
- Local e remotamente usando o Windows PowerShell
- Remotamente usando o [Gerenciador de servidores](../server-manager/server-manager.md)
- Remotamente usando [snap-in MMC](#managing-with-microsoft-management-console)
- Remotamente com [os serviços de área de trabalho remota](#managing-with-remote-desktop-services)

Você também pode adicionar hardware e gerenciar drivers localmente, desde que você fizer isso da linha de comando.

Há algumas limitações importantes e dicas para ter em mente ao trabalhar com núcleo do servidor:

- Se você fecha todas as janelas de prompt de comando e deseja abrir uma nova janela de Prompt de comando, você pode fazer isso no Gerenciador de tarefas. Pressione **CTRL\ + ALT\ + DELETE**, clique em **Iniciar Gerenciador de tarefas** **mais detalhes > arquivo > Executar**e digite **cmd.exe**. (Digite **Powershell.exe** para abrir uma janela de comando do PowerShell). Como alternativa, você pode sair e entrar novamente.
- Qualquer comando ou uma ferramenta que tenta iniciar o Windows Explorer não funcionará. Por exemplo, executando **Iniciar.** em um prompt de comando não funcionarão.
- Não há nenhum suporte para renderização de HTML ou a Ajuda em HTML no núcleo do servidor.
- Núcleo do servidor suporta no Windows Installer em modo silencioso, de modo que você pode instalar as ferramentas e utilitários de arquivos do Windows Installer. Ao instalar os pacotes do Windows Installer no núcleo do servidor, use a opção **/qb** para exibir a interface de usuário básica.
- Para alterar o fuso horário, execute **Set-data**.
- Para alterar as definições internacionais, execute **intl. cpl do controle**.
- **Control.exe** não será executado seu próprio. Você deve executá-lo com **timedate. cpl** ou **intl. cpl**.
- **Winver.exe** não está disponível no núcleo do servidor. Para obter informações sobre a versão use **Systeminfo.exe**.

## <a name="managing-server-core-with-windows-admin-center"></a>Gerenciando o núcleo do servidor com o Centro de administração do Windows
[Centro de administração do Windows](../../manage/windows-admin-center/overview.md) é um aplicativo de gerenciamento baseadas em navegador que permite a administração do local de servidores do Windows com nenhuma dependência do Azure ou nuvem. Centro de administração do Windows oferece controle total sobre todos os aspectos da sua infra-estrutura de servidor e é particularmente útil para gerenciamento de redes privadas que não estão conectados à Internet. Você pode instalar o Centro de administração do Windows no Windows 10, em um servidor de gateway ou em uma instalação do Windows Server com experiência de área de trabalho e, em seguida, conectar ao sistema de núcleo do servidor que você deseja gerenciar.

## <a name="managing-server-core-remotely-with-server-manager"></a>Gerenciando remotamente com o Gerenciador do servidor de núcleo do servidor

Gerenciador de servidores é um console de gerenciamento no Windows Server que ajuda você a provisionar e gerenciar os servidores locais e remotos baseadas no Windows de seus desktops, sem a necessidade de acesso físico aos servidores ou a necessidade de habilitar o protocolo de área de trabalho remota (RDP) conexões a cada servidor. Gerenciador do servidor oferece suporte a gerenciamento remoto, com múltiplos servidor.

Para habilitar o seu servidor local a ser gerenciado pelo Gerenciador de servidores em execução em um servidor remoto, execute o cmdlet do Windows PowerShell **Configure-SMRemoting.exe – habilitar**.

## <a name="managing-with-microsoft-management-console"></a>Gerenciando com o Console de gerenciamento Microsoft

Você pode usar o snap-ins do Console de gerenciamento Microsoft (MMC) muitos remotamente para gerenciar o servidor de núcleo do servidor.

Para usar um snap-in do MMC para gerenciar um servidor de núcleo do servidor que é um membro do domínio: 

1. Inicie um snap-in do MMC, como o gerenciamento do computador.
2. Clique com o botão o snap-in e, em seguida, clique em **Conectar para outro computador**.
2. Digite o nome do computador do servidor de núcleo do servidor e clique em **Okey**. Agora, você pode usar o snap-in do MMC para gerenciar o servidor de núcleo do servidor, como faria com qualquer outro PC ou servidor.

Usar um snap-in do MMC para gerenciar um servidor de núcleo do servidor ou seja *não* um membro do domínio: 

1. Estabeleça credenciais alternativas a serem usadas para se conectar ao computador do núcleo do servidor, digitando o seguinte comando no prompt de comando no computador remoto:
   ```
   cmdkey /add:<ServerName> /user:<UserName> /pass:<password>
   ```
   Se quiser ser solicitado por uma senha, omita a **opção/passar** .

2. Quando solicitado, digite a senha para o nome de usuário que você especificou.
   Se o firewall no servidor de núcleo do servidor já não estiver configurado para permitir que o snap-ins do MMC para se conectar, siga as etapas abaixo para configurar o Firewall do Windows para permitir que o snap-in MMC. Em seguida, continue na etapa 3.
3. Em um computador diferente, inicie um snap-in do MMC, como o **Gerenciamento do computador**.
4. No painel esquerdo, clique com o botão o snap-in e, em seguida, clique em **Conectar para outro computador**. (Por exemplo, o exemplo de gerenciamento do computador, você faria com o botão direito **Gerenciamento do computador (Local)**.)
5. Em **outro computador**, digite o nome do computador do servidor de núcleo do servidor e, em seguida, clique em **Okey**. Agora, você pode usar o snap-in do MMC para gerenciar o servidor de núcleo do servidor, como faria com qualquer outro computador executando o sistema operacional Windows Server.

### <a name="to-configure-windows-firewall-to-allow-mmc-snap-ins-to-connect"></a>Para configurar o Firewall do Windows para permitir que o snap-in do MMC (s) para se conectar
Para permitir que todos os snap-ins MMC para se conectar, execute o seguinte comando:

```
Enable-NetFirewallRule -DisplayGroup "Remote Administration"
```

Para permitir que somente MMC snap-ins específicos para se conectar, execute o seguinte:
```
Enable-NetFirewallRule -DisplayGroup "<rulegroup>"
```

Onde *rulegroup* é um dos seguintes procedimentos, dependendo de qual snap-in você deseja se conectar:

| Snap-in MMC                            | Grupo de regras                                            |
|----------------------------------------|-------------------------------------------------------|
| Visualizador de Eventos                           | Gerenciamento remoto de Log de eventos                           |
| Serviços                               | Gerenciamento de serviço remoto                             |
| Pastas compartilhadas                         | Compartilhamento de arquivo e impressora                              |
| Agendador de Tarefas                         | Logs de desempenho e alertas, compartilhamento de arquivo e impressora |
| Gerenciamento de disco                        | Gerenciamento de volumes remoto                              |
| Firewall do Windows e segurança avançada | Gerenciamento remoto do Windows Firewall                    |


> [!NOTE] 
> Alguns snap-ins do MMC não têm um grupo de regra correspondente que permita a conexão através do firewall. No entanto, habilitando os grupos de regras para o Visualizador de eventos, serviços ou pastas compartilhadas permitirá que a maioria dos outros snap-ins para se conectar. 
>
> Além disso, certos snap-ins exige a configuração adicional antes que possam se conectar por meio do Firewall do Windows:
>
> - Gerenciamento de disco. Primeiro, você deve iniciar o serviço de disco Virtual (VDS) no computador de núcleo do servidor. Você também deve configurar as regras de gerenciamento de disco apropriadamente no computador que está executando o snap-in do MMC.
> - Monitor de segurança IP. Você deve primeiro habilitar o gerenciamento remoto do snap-in. Para fazer isso, no prompt de comando, digite **Cscript \windows\system32\scregedit.wsf /im 1**
> - Confiabilidade e desempenho. O snap-in não exige qualquer outra configuração, mas quando você usá-lo para monitorar um computador de núcleo do servidor, você só pode monitorar dados de desempenho. Dados de confiabilidade não estão disponíveis.

## <a name="managing-with-remote-desktop-services"></a>Gerenciando com os serviços de área de trabalho remota

Você pode usar a [Área de trabalho remota](../../remote/remote-desktop-services/welcome-to-rds.md) para gerenciar um servidor de núcleo do servidor de computadores remotos.

Antes de poder acessar núcleo do servidor, você precisará executar o seguinte comando: 
```
cscript C:\Windows\System32\Scregedit.wsf /ar 0
```
Isso permite que a área de trabalho remota para o modo de administração aceitar conexões.

## <a name="add-hardware-and-manage-drivers-locally"></a>Adicionar hardware e gerenciar drivers localmente

Para adicionar hardware para um servidor de núcleo do servidor, siga as instruções fornecidas pelo fornecedor de hardware para instalar o novo hardware. 

Se o hardware não for plug and play, você precisará instalar manualmente o driver. Para fazer isso, copie os arquivos de driver para um local temporário no servidor e, em seguida, execute o seguinte comando:
```
pnputil –i –a <driverinf>
```
Onde *driverinf* é o nome de arquivo do arquivo. inf para o driver.

Se solicitado, reinicie o computador.

Para ver quais drivers estão instalados, execute o seguinte comando: 
```
sc query type= driver
```

> [!NOTE] 
> Você deve incluir o espaço após o sinal de igualdade do comando seja concluído com êxito.

Para desabilitar um driver de dispositivo, execute o seguinte: 
```
sc delete <service_name>
```

Onde *service_name* é o nome do serviço que você obteve quando executou **tipo de consulta sc = driver**.