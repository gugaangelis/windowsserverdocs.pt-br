---
title: Copie o certificado de autoridade de certificação e a CRL para o diretório Virtual
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/19/2018
ms.openlocfilehash: 15cc807db805e1be0349ea51119663e515c7e551
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874027"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>Copie o certificado de autoridade de certificação e a CRL para o diretório Virtual

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para copiar o certificado de CA raiz empresarial e de lista de revogação de certificado da autoridade de certificação para um diretório virtual em seu servidor Web e para garantir que o AD CS esteja configurado corretamente. Antes de executar os comandos a seguir, certifique-se de que você substituir os nomes de diretório e o servidor com aquelas que são apropriadas para sua implantação.  
  
Para executar este procedimento, você deve ser um membro da **Admins. do domínio**.  
  
#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>Para copiar a lista de revogação de certificados de CA1 para WEB1  
  
1.  No CA1, execute o Windows PowerShell como administrador e, em seguida, publicar a CRL com o seguinte comando:  
  
    - Digite `certutil -crl` e pressione ENTER.  

    - Para copiar as listas de revogação de certificado para o compartilhamento de arquivos em seu servidor Web, digite `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki`, e pressione ENTER.  
  
2.  Para verificar se seus locais de extensão do CPD e AIA estão configurados corretamente, digite `pkiview.msc`, e pressione ENTER. A pkiview Enterprise PKI MMC é aberto.  
  
3.  No painel esquerdo, clique no nome de autoridade de certificação.<p>Por exemplo, se o nome de autoridade de certificação for corp-CA1-CA, clique em **corp-CA1-CA**. 

4. Na coluna Status do painel de resultados, verifique se os valores para o seguinte mostra **Okey**:

    - **Certificado de autoridade de certificação**
    - **Local AIA #1**
    - **Local de CPD #1**   
  
  
> [!TIP]  
> Se **Status** para qualquer item não é **Okey**, faça o seguinte:  
> -   Abra o compartilhamento no servidor Web para verificar o certificado e os arquivos de lista de revogação de certificado foram copiados com êxito para o compartilhamento. Se eles não foram copiados com êxito para o compartilhamento, modificar os comandos de cópia com a fonte de arquivo correto e compartilhar o destino e execute os comandos novamente.  
> -   Verifique se você inseriu os locais corretos de CPD e AIA na guia Extensões de autoridade de certificação. Verifique se não há nenhum espaço adicional ou outros caracteres nos locais que você forneceu.  
> -   Verifique se que você copiou o certificado de autoridade de certificação da CRL para o local correto em seu servidor Web, e se o local corresponde ao local fornecido para os locais de CPD e AIA na autoridade de certificação.  
> -   Verifique se você configurou corretamente as permissões para a pasta virtual onde o certificado de autoridade de certificação e a CRL são armazenados.  
  


