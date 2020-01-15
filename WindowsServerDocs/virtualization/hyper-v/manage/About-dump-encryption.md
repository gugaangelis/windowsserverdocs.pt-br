---
title: Sobre a criptografia de despejo
description: Descreve como criptografar arquivos de despejo e solucionar problemas de criptografia.
ms.prod: windows-server
manager: dongill
ms.topic: article
author: larsiwer
ms.asset: b78ab493-e7c3-41f5-ab36-29397f086f32
ms.author: kathydav
ms.date: 11/03/2016
ms.openlocfilehash: 2232f62090e171060f25e4c2513a217e2ab98eaa
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950545"
---
# <a name="about-dump-encryption"></a>Sobre a criptografia de despejo
A criptografia de despejo pode ser usada para criptografar despejos de memória e despejos dinâmicos gerados para um sistema. Os despejos são criptografados usando uma chave de criptografia simétrica que é gerada para cada despejo. Em seguida, essa chave é criptografada usando a chave pública especificada pelo administrador confiável do host (protetor de chave de criptografia de despejo de memória). Isso garante que apenas alguém que tenha a chave privada correspondente possa descriptografar e, portanto, acessar o conteúdo do despejo. Esse recurso é utilizado em uma malha protegida.
Observação: se você configurar a criptografia de despejo, também desabilite Relatório de Erros do Windows. O WER não pode ler despejos de memória criptografados.

## <a name="configuring-dump-encryption"></a>Configurando a criptografia de despejo
### <a name="manual-configuration"></a>Configuração manual
Para ativar a criptografia de despejo usando o registro, configure os seguintes valores de registro em `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl`

| Nome do valor | Digite | Valor |
| ---------- | ---- | ----- |
| DumpEncryptionEnabled | DWORD | 1 para habilitar a criptografia de despejo, 0 para desabilitar a criptografia de despejo |
| EncryptionCertificates\Certificate.1::P ublicKey | Binário | Chave pública (RSA, 2048 bits) que deve ser usada para criptografar despejos. Isso deve ser formatado como [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| EncryptionCertificates\Certificate.1:: impressão digital | Cadeia de caracteres | Impressão digital do certificado para permitir a pesquisa automática de chave privada no repositório de certificados local ao descriptografar um despejo de memória. |


### <a name="configuration-using-script"></a>Configuração usando script
Para simplificar a configuração, um [script de exemplo](https://github.com/Microsoft/Virtualization-Documentation/tree/live/hyperv-tools/DumpEncryption) está disponível para habilitar a criptografia de despejo com base em uma chave pública de um certificado.

1. Em um ambiente confiável: Crie um certificado com uma chave RSA de 2048 bits e exporte o certificado público
2. Em hosts de destino: importe o certificado público para o repositório de certificados local
3. Executar o script de configuração de exemplo 
    ```
    .\Set-DumpEncryptionConfiguration.ps1 -Certificate (Cert:\CurrentUser\My\093568AB328DF385544FAFD57EE53D73EFAAF519) -Force
    ```

## <a name="decrypting-encrypted-dumps"></a>Descriptografando despejos criptografados
Para descriptografar um arquivo de despejo criptografado existente, você precisa baixar e instalar as ferramentas de depuração para Windows. Esse conjunto de ferramentas contém KernelDumpDecrypt. exe, que pode ser usado para descriptografar um arquivo de despejo criptografado.
Se o certificado que inclui a chave privada estiver presente no repositório de certificados do usuário atual, o arquivo de despejo poderá ser descriptografado chamando

```
    KernelDumpDecrypt.exe memory.dmp memory_decr.dmp
```
Após a descriptografia, ferramentas como o WinDbg podem abrir o arquivo de despejo descriptografado.

## <a name="troubleshooting-dump-encryption"></a>Solucionando problemas de criptografia de despejo
Se a criptografia de despejo estiver habilitada em um sistema, mas nenhum despejo estiver sendo gerado, verifique o log de eventos `System` do sistema para `Kernel-IO` evento 1207. Quando a criptografia de despejo não puder ser inicializada, esse evento será criado e os despejos serão desabilitados.

| Mensagem de erro detalhada | Etapas para mitigar |
| ---------------------- | ----------------- |
| Registro de chave pública ou impressão digital ausente | Verificar se ambos os valores do registro existem no local esperado |
| Chave pública inválida | Verifique se a chave pública armazenada no valor do registro PublicKey está armazenada como [BCRYPT_RSAKEY_BLOB](https://msdn.microsoft.com/library/windows/desktop/aa375531(v=vs.85).aspx). |
| Tamanho de chave pública sem suporte | Atualmente, há suporte apenas para chaves RSA de 2048 bits. Configurar uma chave que corresponda a esse requisito |

Verifique também se o valor `GuardedHost` em `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled` está definido com um valor diferente de 0. Isso desabilita completamente os despejos de memória. Se esse for o caso, defina-o como 0.
