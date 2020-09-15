---
title: Configurar arquivos de despejo de memória para a instalação do Server Core
description: Saiba como configurar arquivos de despejo de memória para uma instalação do Server Core do Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: pronichkin
ms.author: artemp
ms.localizationpriority: medium
ms.date: 10/17/2017
ms.openlocfilehash: e3ef6076465bc7d165b58f1205ff8d0cf25014b7
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077773"
---
# <a name="configure-memory-dump-files-for-server-core-installation"></a>Configurar arquivos de despejo de memória para a instalação do Server Core

>Aplica-se a: Windows Server 2019, Windows Server 2016 e Windows Server (canal semestral)

Use as etapas a seguir para configurar um despejo de memória para a instalação do Server Core.

## <a name="step-1-disable-the-automatic-system-page-file-management"></a>Etapa 1: desabilitar o gerenciamento de arquivos de página de sistema automático

A primeira etapa é configurar manualmente as opções de falha e recuperação do sistema. Isso é necessário para concluir as etapas restantes.

Execute o comando a seguir:

```
wmic computersystem set AutomaticManagedPagefile=False
```

## <a name="step-2-configure-the-destination-path-for-a-memory-dump"></a>Etapa 2: configurar o caminho de destino para um despejo de memória

Você não precisa ter o arquivo de paginação na partição em que o sistema operacional está instalado. Para colocar o arquivo de paginação em outra partição, você deve criar uma nova entrada de registro chamada **DedicatedDumpFile**. Você pode definir o tamanho do arquivo de paginação usando a entrada do registro **DumpFileSize** . Para criar as entradas de registro DedicatedDumpFile e DumpFileSize, siga estas etapas:

1. No prompt de comando, execute o comando **regedit** para abrir o editor do registro.
2. Localize e clique na seguinte subchave do registro: HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Control\CrashControl
3. Clique em **editar > novo > valor da cadeia de caracteres**.
4. Nomeie o novo valor **DedicatedDumpFile**e pressione Enter.
5. Clique com o botão direito do mouse em **DedicatedDumpFile**e clique em **Modificar**.
6. Em tipo de **dados de valor** ** \<Drive\> \\ \<Dedicateddumpfile.sys\> :**, e clique em **OK**.

   >[!NOTE]
   > Substitua \<Drive\> por uma unidade que tenha espaço em disco suficiente para o arquivo de paginação e substitua \<Dedicateddumpfile.dmp\> pelo caminho completo do arquivo dedicado.

7. Clique em **editar > novo > valor DWORD**.
8. Digite **DumpFileSize**e pressione Enter.
9. Clique com o botão direito do mouse em **DumpFileSize**e clique em **Modificar**.
10. Em **Editar valor DWORD**, em **base**, clique em **decimal**.
11. Em **dados de valor**, digite o valor apropriado e clique em **OK**.
    >[!NOTE]
    > O tamanho do arquivo de despejo é em megabytes (MB).
12. Saia do editor do registro.

Depois de determinar o local da partição do despejo de memória, configure o caminho de destino para o arquivo de paginação. Para exibir o caminho de destino atual para o arquivo de paginação, execute o seguinte comando:

```
wmic RECOVEROS get DebugFilePath
```

O destino padrão para **DebugFilePath** é%SystemRoot%\Memory.dmp. Para alterar o caminho de destino atual, execute o seguinte comando:

```
wmic RECOVEROS set DebugFilePath = <FilePath>
```

Defina \<FilePath\> como o caminho de destino. Por exemplo, o comando a seguir define o caminho de destino de despejo de memória para C:\WINDOWS\MEMORY. DMP

```
wmic RECOVEROS set DebugFilePath = C:\WINDOWS\MEMORY.DMP
```

## <a name="step-3-set-the-type-of-memory-dump"></a>Etapa 3: definir o tipo de despejo de memória

Determine o tipo de despejo de memória a ser configurado para o servidor. Para exibir o tipo de despejo de memória atual, execute o seguinte comando:

```
wmic RECOVEROS get DebugInfoType
```

Para alterar o tipo de despejo de memória atual, execute o seguinte comando:

```
wmic RECOVEROS set DebugInfoType = <Value>
```

\<Value\> pode ser 0, 1, 2 ou 3, conforme definido abaixo.

- 0: desabilitar a remoção de um despejo de memória.
- 1: despejo de memória completo. Registra todo o conteúdo da memória do sistema quando o computador é interrompido inesperadamente. Um despejo de memória cheio pode conter dados de processos que estavam em execução quando o despejo de memória foi coletado.
- 2: despejo de memória do kernel (padrão). Registra somente a memória do kernel. Isso acelera o processo de gravação de informações em um arquivo de log quando o computador parar inesperadamente.
- 3: despejo de memória pequeno. Registra o menor conjunto de informações úteis que podem ajudar a identificar por que o computador parou inesperadamente.

## <a name="step-4-configure-the-server-to-restart-automatically-after-generating-a-memory-dump"></a>Etapa 4: configurar o servidor para reiniciar automaticamente após gerar um despejo de memória

Por padrão, o servidor é reiniciado automaticamente após gerar um despejo de memória. Para exibir a configuração atual, execute o seguinte comando:

```
wmic RECOVEROS get AutoReboot
```

Se o valor de **reinicialização** automática for true, o servidor será reiniciado automaticamente após gerar um despejo de memória. Nenhuma configuração é necessária e você pode prosseguir para a próxima etapa.

Se o valor de **reinicialização** automática for false, o servidor não será reiniciado automaticamente. Execute o seguinte comando para alterar o valor:

```
wmic RECOVEROS set AutoReboot = true
```

## <a name="step-5-configure-the-server-to-overwrite-the-existing-memory-dump-file"></a>Etapa 5: configurar o servidor para substituir o arquivo de despejo de memória existente

Por padrão, o servidor substitui o arquivo de despejo de memória existente quando um novo é criado. Para determinar se os arquivos de despejo de memória já estão configurados para serem substituídos, execute o seguinte comando:

```
wmic RECOVEROS get OverwriteExistingDebugFile
```

Se o valor for 1, o servidor substituirá o arquivo de despejo de memória existente. Nenhuma configuração é necessária e você pode prosseguir para a próxima etapa.

Se o valor for 0, o servidor não substituirá o arquivo de despejo de memória existente. Execute o seguinte comando para alterar o valor:

```
wmic RECOVEROS set OverwriteExistingDebugFile = 1
```

## <a name="step-6-set-an-administrative-alert"></a>Etapa 6: definir um alerta administrativo

Determine se um alerta administrativo é apropriado e defina **SendAdminAlert** de acordo. Para exibir o valor atual de SendAdminAlert, execute o seguinte comando:

```
wmic RECOVEROS get SendAdminAlert
```

Os valores possíveis para SendAdminAlert são TRUE ou FALSE. Para modificar o valor de SendAdminAlert existente para true, execute o seguinte comando:

```
wmic RECOVEROS set SendAdminAlert = true
```

## <a name="step-7-set-the-memory-dumps-page-file-size"></a>Etapa 7: definir o tamanho do arquivo de paginação do despejo de memória

Para verificar as configurações do arquivo de paginação atual, execute um dos seguintes comandos:

   ```
   wmic.exe pagefile
   ```

   ou

   ```
   wmic.exe pagefile list /format:list
   ```

Por exemplo, execute o seguinte comando para configurar os tamanhos inicial e máximo do arquivo de paginação:

```
wmic pagefileset where name="c:\\pagefile.sys" set InitialSize=1000,MaximumSize=5000
```

## <a name="step-8-configure-the-server-to-generate-a-manual-memory-dump"></a>Etapa 8: configurar o servidor para gerar um despejo de memória manual

Você pode gerar manualmente um despejo de memória usando um teclado PS/2. Esse recurso está desabilitado por padrão e não está disponível para teclados de barramento serial universal (USB).

Para habilitar os despejos de memória manuais usando um teclado PS/2, execute o seguinte comando:

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\i8042prt\Parameters /v CrashOnCtrlScroll /t REG_DWORD /d 1 /f
```

Para determinar se o recurso foi habilitado corretamente, execute o seguinte comando:

```
Reg query HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ i8042prt \ Parameters / v CrashOnCtrlScroll
```

Você deve reiniciar o servidor para que as alterações entrem em vigor. Você pode reiniciar o servidor executando o seguinte comando:

```
Shutdown / r / t 0
```

Você pode gerar despejos de memória manuais com um teclado PS/2 conectado ao servidor mantendo a tecla CTRL direita enquanto pressiona a tecla de bloqueio de ROLAgem duas vezes. Isso faz com que a verificação de bug do computador tenha o código de erro 0xE2. Depois de reiniciar o servidor, um novo arquivo de despejo é exibido no caminho de destino que você estabeleceu na etapa 2.

## <a name="step-9-verify-that-memory-dump-files-are-being-created-correctly"></a>Etapa 9: verificar se os arquivos de despejo de memória estão sendo criados corretamente

Você pode usar o dumpchk.exe utlity para verificar se os arquivos de despejo de memória estão sendo criados corretamente. O utilitário dumpchk.exe não está instalado com a opção de instalação Server Core, portanto, você precisará executá-lo em um servidor com a experiência desktop ou com o Windows 10. Além disso, as ferramentas de depuração para produtos Windows devem ser instaladas.

O utilitário dumpchk.exe permite transferir o arquivo de despejo de memória da instalação Server Core do Windows Server 2008 para o outro computador usando a mídia de sua escolha.

> [!WARNING]
> Os arquivos de paginação podem ser muito grandes, portanto, considere cuidadosamente o método de transferência e os recursos que o método exige.


Referências adicionais

Para obter informações gerais sobre como usar arquivos de despejo de memória, consulte [visão geral das opções de arquivo de despejo de memória para Windows](https://support.microsoft.com/help/254649/overview-of-memory-dump-file-options-for-windows).

Para obter mais informações sobre arquivos de despejo dedicados, consulte [como usar o valor do registro DedicatedDeumpFile para superar as limitações de espaço na unidade do sistema durante a captura de um despejo de memória do sistema](/archive/blogs/ntdebugging/how-to-use-the-dedicateddumpfile-registry-value-to-overcome-space-limitations-on-the-system-drive-when-capturing-a-system-memory-dump).