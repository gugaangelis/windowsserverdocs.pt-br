---
title: Ao se conectar, o usuário recebe a mensagem que informa que o Serviço de Área de Trabalho Remota está ocupado no momento
description: Solução do erro “O Serviço de Área de Trabalho Remota está ocupado no momento” quando o usuário inicia uma conexão de área de trabalho remota.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: ''
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 889fd83925081ac1dce386b1cd18fbef59586eb5
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68529896"
---
# <a name="on-connecting-user-receives-remote-desktop-service-is-currently-busy-message"></a>Ao se conectar, o usuário recebe uma mensagem "O serviço de Área de Trabalho Remota está ocupado no momento"

Para determinar uma resposta apropriada para esse problema, veja o seguinte:

- O serviço dos Serviços de Área de Trabalho Remota para de responder (por exemplo, o cliente de Área de Trabalho Remota parece "congelar" na tela de boas-vindas).  
   - Se o serviço deixar de responder, confira [Problema de memória do servidor RDSH](#rdsh-server-memory-issue).
   - Se o cliente parece estar interagindo normalmente com o serviço, prossiga para a próxima etapa.
- Se um ou mais usuários desconectarem as respectivas sessões de Área de Trabalho Remota, eles poderão se conectar novamente?  
   - Se o serviço continuar a negar conexões independentemente de quantos usuários desconectam suas sessões, confira [Problema de ouvinte de Área de Trabalho Remota](#rd-listener-issue).
   - Se o serviço começar a aceitar conexões novamente após um número de usuários terem suas sessões desconectadas, [verifique a política de limite de conexões](#check-the-connection-limit-policy).

## <a name="rdsh-server-memory-issue"></a>Problema de memória do servidor RDSH

Foi encontrado um vazamento de memória em alguns servidores do Windows Server 2012 R2 RDSH. Ao longo do tempo, esses servidores começam a recusar conexões de área de trabalho remota e entradas do console local com mensagens como a seguinte:

> A tarefa que você está tentando realizar não pode ser concluída porque o Serviço de Área de Trabalho Remota está ocupado no momento. Tente novamente em alguns minutos. Outros usuários ainda devem ser capazes de entrar.

Clientes da Área de Trabalho Remota tentando se conectar também param de responder.

Para contornar esse problema, reinicie o servidor RDSH.

Para resolver esse problema, aplique KB 4093114, [10 de abril de 2018—KB4093114 (Pacote cumulativo de atualizações mensal)](https://support.microsoft.com/help/4093114/), aos servidores RDSH.

## <a name="rd-listener-issue"></a>Problema de ouvinte de Área de Trabalho Remota

Um problema foi observado em alguns servidores RDSH que foram atualizados diretamente do Windows Server 2008 R2 para o Windows Server 2012 R2 ou o Windows Server 2016. Quando um cliente de Área de Trabalho Remota se conecta ao servidor RDSH, o servidor RDSH cria um ouvinte de área de trabalho remota para a sessão do usuário. Os servidores afetados mantêm uma contagem dos ouvintes de Área de Trabalho Remota que aumenta à medida que os usuários se conectam, mas nunca diminui.

É possível solucionar esse problema com os métodos a seguir:

  - Reiniciar o servidor RDSH para redefinir a contagem de ouvintes de Área de Trabalho Remota
  - Modificar a política de limite de conexões, definindo-a como um valor muito grande. Para obter mais informações sobre como gerenciar a política de limite de conexões, confira [Verificar a política de limite de conexões](#check-the-connection-limit-policy).

Para resolver esse problema, aplique as seguintes atualizações aos servidores RDSH:

  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018 – KB4343891 (versão prévia do pacote cumulativo de atualizações mensal)](https://support.microsoft.com/help/4343891/windows-81-update-kb4343891)
  - Windows Server 2016: KB 4343884, [30 de agosto de 2018 – KB4343884 (Build do sistema operacional 14393.2457)](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)

## <a name="check-the-connection-limit-policy"></a>Verifique a política de limite de conexões

Você pode definir o limite para o número de conexões simultâneas de Área de Trabalho Remota no nível de computador individual ou por meio da configuração de um GPO (objeto de política de grupo). Por padrão, o limite não é definido.

Para verificar as configurações atuais e identificar quaisquer GPOs existentes no servidor de RDSH, abra uma janela de prompt de comando como administrador e digite o seguinte comando:
  
```cmd
gpresult /H c:\gpresult.html
```
   
Após a conclusão desse comando, abra **gpresult.html**. Em **Configuração do Computador\\Modelos Administrativos\\Componentes do Windows\\Serviços de Área de Trabalho Remota\\Host da Sessão da Área de Trabalho Remota\\Conexões**, localize a política **Limitar o número de conexões**.

  - Se a configuração para essa política está **Desabilitada**, a Política de Grupo não está limitando as conexões RDP.
  - Se a configuração para essa política estiver **Habilitada**, verifique o **GPO vencedor**. Se você precisar remover ou alterar o limite de conexões, edite esse GPO.

Para aplicar alterações de política, abra uma janela de prompt de comando no computador afetado e digite o seguinte comando:
  
```cmd
gpupdate /force
```
  
