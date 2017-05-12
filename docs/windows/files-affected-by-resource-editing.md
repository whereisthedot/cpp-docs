---
title: "Files Affected by Resource Editing | Microsoft Docs"
ms.custom: ""
ms.date: "11/04/2016"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-cpp"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "C++"
helpviewer_keywords: 
  - "resource editing"
  - "resources [Visual Studio], editing"
ms.assetid: d0820df1-ba84-40ac-bce9-29ea5ee7e3f8
caps.latest.revision: 7
author: "mikeblome"
ms.author: "mblome"
manager: "ghogen"
translation.priority.ht: 
  - "cs-cz"
  - "de-de"
  - "es-es"
  - "fr-fr"
  - "it-it"
  - "ja-jp"
  - "ko-kr"
  - "pl-pl"
  - "pt-br"
  - "ru-ru"
  - "tr-tr"
  - "zh-cn"
  - "zh-tw"
---
# Files Affected by Resource Editing
The Visual Studio environment works with the files shown in the following table during your resource editing session.  
  
|File name|Description|  
|---------------|-----------------|  
|Resource.h|Header file generated by the development environment; contains symbol definitions.|  
|Filename.aps|Binary version of the current resource script file; used for quick loading.<br /><br /> The resource editors do not directly read .rc or resource.h files. The resource compiler compiles them into .aps files, which are consumed by the resource editors. This file is a compile step and only stores symbolic data. As with a normal compile process, information that is not symbolic (for example, comments) is discarded during the compile process. Whenever the .aps file gets out of synch with the .rc file, the .rc file is regenerated (for example, when you Save, the resource editor overwrites the .rc file and the resource.h file). Any changes to the resources themselves will remain incorporated in the .rc file, but comments will always be lost once the .rc file is overwritten. For information on how to preserve comments, see [Including Resources at Compile Time](../windows/how-to-include-resources-at-compile-time.md).|  
|.rc|Resource script file that contains script for the resources in your current project. This file is overwritten by the .aps file whenever you save.|  
  

  
## Requirements  
 Win32  
  
## See Also  
 [Resource Files](../windows/resource-files-visual-studio.md)