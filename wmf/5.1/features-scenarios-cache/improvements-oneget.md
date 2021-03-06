---
title: "Усовершенствования модуля OneGet в WMF 5.1 (предварительная версия)"
ms.date: 2016-07-13
keywords: PowerShell, DSC, WMF
description: 
ms.topic: article
author: keithb
manager: dongill
ms.prod: powershell
ms.technology: WMF
translationtype: Human Translation
ms.sourcegitcommit: 57049ff138604b0e13c8fd949ae14da05cb03a4b
ms.openlocfilehash: 1d0bd545b52ef56045f2ec740b05c4e0fd93bb67

---

# Усовершенствования модуля OneGet
В WMF 5.1 внесен ряд исправлений и улучшений с целью устранить некоторые проблемы, возникавшие у пользователей при работе с версией WMF 5.0. 

##Удаление псевдонима Version

**Сценарий**. Если в системе установлены версии 1.0 и 2.0 пакета P1 и вы хотите удалить версию 1.0, то вы выполняете команду "uninstall-package -name P1 -version 1.0". При этом вы ожидаете, что после выполнения командлета будет удалена версия 1.0. Но в результате удаляется версия 2.0. 
    
Это происходит потому, что параметр -version является псевдонимом параметра -minimumversion. Когда модуль OneGet ищет подходящий пакет с минимальной версией 1.0, он возвращает последнюю версию. Такое поведение является нормальным в большинстве случаев, так как обычно требуется найти именно последнюю версию. Но в случае с удалением пакета ситуация иная.
    
**Решение**. В WMF 5.1 псевдоним -version полностью удален из модулей OneGet и PowerShellGet. 

##Несколько запросов на начальную загрузку поставщика NuGet

**Сценарий**. При первом выполнении командлета Find-Module, Install-Module или других командлетов OneGet на компьютере модуль OneGet пытается выполнить начальную загрузку поставщика NuGet. Связано это с тем, что поставщик PowershellGet также использует поставщик NuGet для скачивания модулей PowerShell. Затем модуль OneGet запрашивает у пользователя разрешение на установку поставщика NuGet. После того как пользователь разрешает начальную загрузку, устанавливается последняя версия поставщика NuGet. 
    
Но если на компьютере установлена старая версия поставщика NuGet, она иногда может загружаться первой в сеанс PowerShell (так как в OneGet возникает состояние гонки). Но модуль PowerShellGet требует, чтобы работала последняя версия поставщика NuGet, поэтому он еще раз запрашивает начальную загрузку поставщика NuGet у модуля OneGet. Это приводит к выводу нескольких запросов на начальную загрузку поставщика NuGet.

**Решение**. В WMF 5.1 модуль OneGet теперь загружает последнюю версию поставщика NuGet во избежание вывода нескольких запросов на начальную загрузку поставщика NuGet.

Также имеется обходной путь: вы можете вручную удалить старую версию поставщика NuGet (NuGet-Anycpu.exe), если она существует, из папок $env:ProgramFiles\PackageManagement\ProviderAssemblies и $env:LOCALAPPDATA\PackageManagement\ProviderAssemblies


##Поддержка OneGet на компьютерах с доступом только к интрасети

**Сценарий**. В WMF 5.0 модуль OneGet не поддерживался на компьютерах с доступом только к интрасети (но не к Интернету).

**Решение**. Чтобы обеспечить использование OneGet на компьютерах в интрасети, в WMF 5.1 можно выполнить указанные ниже действия.

1. Скачайте поставщик NuGet с помощью другого компьютера, имеющего подключение к Интернету, выполнив команду "Install-PackageProvider NuGet".

2. Поставщик NuGet находится в папке $env:ProgramFiles\PackageManagement\ProviderAssemblies\nuget или $env:LOCALAPPDATA\PackageManagement\ProviderAssemblies\nuget. 

3. Скопируйте двоичные файлы в папку или сетевую папку, к которой есть доступ у компьютера в интрасети, и установите поставщик NuGet, выполнив команду "Install-PackageProvider NuGet -Source <Path to folder>".


##Усовершенствования, касающиеся ведения журнала событий

При установке пакетов состояние компьютера меняется. В WMF 5.1 модуль OneGet теперь записывает в журнал событий Windows события, связанные с установкой, удалением и сохранением пакетов. Канал событий тот же, что и для PowerShell, то есть Microsoft-Windows-PowerShell, Operational.

##Поддержка обычной проверки подлинности

В WMF 5.1 модуль OneGet поддерживает поиск и установку пакетов из репозитория, требующего обычной проверки подлинности. Вы можете указывать учетные данные для командлетов Find-Package и Install-Package. Например:

``` PowerShell
Find-Package -Source <SourceWithCredential> -Credential (Get-Credential)
```
##Поддержка использования OneGet за прокси-сервером

В WMF 5.1 модуль OneGet теперь принимает новые параметры прокси-сервера: -ProxyCredential и -Proxy. С помощью этих параметров можно указать URL-адрес и учетные данные прокси-сервера для командлетов OneGet. По умолчанию используются системные настройки прокси-сервера. Например:

``` PowerShell
Find-Package -Source http://www.nuget.org/api/v2/ -Proxy http://www.myproxyserver.com -ProxyCredential (Get-Credential)
```



<!--HONumber=Jul16_HO3-->


