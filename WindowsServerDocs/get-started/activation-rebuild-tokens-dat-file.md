---
title: Recompilar o arquivo Tokens.dat
description: Como recompilar o arquivo Tokens.dat quando você soluciona problemas de ativação do Windows
ms.topic: troubleshooting
ms.date: 09/18/2019
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: bc44dae97422e4d9d9e55b32004f806bbb7860f7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941778"
---
# <a name="rebuild-the-tokensdat-file"></a>Recompilar o arquivo Tokens.dat

Quando você soluciona problemas de ativação do Windows, pode precisar recompilar o arquivo Tokens.dat. Este artigo descreve detalhadamente como fazer isso.

## <a name="resolution"></a>Resolução

Para recompilar o arquivo Tokens.dat, siga estas etapas:

1. Abra uma janela do prompt de comandos com privilégios elevados: **Para Windows 10**

   1. Abra o menu **Iniciar** e insira **cmd**.
   1. Nos resultados da pesquisa, clique com o botão direito do mouse em **Prompt de Comando** e selecione **Executar como administrador**.

   **Para Windows 8.1**
   1. Passe o dedo da borda direita da tela e toque em **Pesquisar**. Ou, se você estiver usando um mouse, aponte para o canto inferior direito da tela e selecione **Pesquisar**.
   1. Na caixa de pesquisa, digite **cmd**.
   1. Deslize o dedo ou clique com o botão direito do mouse no ícone **Prompt de Comando** exibido.
   1. Toque ou clique em **Executar como administrador**.

   **Para Windows 7**
   1. Abra o menu **Iniciar** e insira **cmd**.
   1. Na lista de resultados da pesquisa, clique com o botão direito do mouse em **cmd.exe** e selecione **Executar como administrador**.

1. Insira a lista de comandos adequada para seu sistema operacional.

   Para o Windows 10, o Windows Server 2016 e as versões posteriores do Windows, insira os seguintes comandos em sequência:
   ```cmd
   net stop sppsvc
   cd %windir%\system32\spp\store\2.0
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
   Para o Windows 8.1, o Windows Server 2012 e o Windows Server 2012 R2, insira os seguintes comandos em sequência:
   ```cmd
   net stop sppsvc
   cd %windir%\ServiceProfiles\LocalService\AppData\Local\Microsoft\WSLicense
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
   Para o Windows 7, o Windows Server 2008 e o Windows Server 2008 R2, insira os seguintes comandos em sequência:
   ```cmd
   net stop sppsvc
   cd %windir%\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft\SoftwareProtectionPlatform
   ren tokens.dat tokens.bar
   net start sppsvc
   cscript.exe %windir%\system32\slmgr.vbs /rilc
   ```
1. Reinicie o computador.

## <a name="more-information"></a>Mais informações

Após recompilar o arquivo Tokens.dat, é necessário reinstalar sua chave do produto (Product Key) usando um dos seguintes métodos:

- No mesmo prompt de comando com privilégios elevados, digite o seguinte comando e pressione Enter:

   ```cmd
   cscript.exe %windir%\system32\slmgr.vbs /ipk <Product key>
   ```

  > [!IMPORTANT]
  > Não use a opção **/upk** para desinstalar uma chave do produto (Product Key). Para instalar uma chave do produto (Product Key) em uma chave do produto (Product Key) existente, use a opção **/ipk**.
- Clique com o botão direito do mouse em **Meu Computador**, selecione **Propriedades** e selecione **Alterar chave do produto (Product Key)** .

Para saber mais sobre chaves de instalação de cliente KMS, confira [Chaves de instalação de cliente KMS](kmsclientkeys.md).
