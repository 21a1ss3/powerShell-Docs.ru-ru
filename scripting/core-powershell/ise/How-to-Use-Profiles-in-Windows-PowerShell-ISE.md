---
title: "Использование профилей в интегрированной среде сценариев Windows PowerShell"
ms.date: 2016-05-11
keywords: "powershell,командлет"
description: 
ms.topic: article
author: jpjofre
manager: dongill
ms.prod: powershell
ms.assetid: 0219626a-6da5-4acc-b630-d058e8b29cc6
translationtype: Human Translation
ms.sourcegitcommit: 3222a0ba54e87b214c5ebf64e587f920d531956a
ms.openlocfilehash: d934eac30da3d6a180d0dd92194eedbfd4cd8844

---

# Использование профилей в интегрированной среде сценариев Windows PowerShell
В этом разделе объясняется, как использовать профили в интегрированной среде сценариев Windows PowerShellÂ®. Перед выполнением задач из этого раздела рекомендуется ознакомиться со статьей [about_Profiles [v4]](https://technet.microsoft.com/en-us/library/e1d9e30a-70cc-4f36-949f-fc7cd96b4054) либо ввести get-help about_profiles в области консоли и нажать клавишу **ВВОД**.

Профиль — это сценарий интегрированной среды сценариев Windows PowerShell, который выполняется автоматически при запуске нового сеанса.  Можно создать один или несколько профилей Windows PowerShell для интегрированной среды сценариев Windows PowerShell и использовать их для настройки среды Windows PowerShell для интегрированной среды сценариев Windows PowerShell, подготавливая ее к работе с помощью переменных, псевдонимов, функций, а также настроек цветов и шрифтов, которые должны быть доступны. Профиль затрагивает каждый запускаемый сеанс интегрированной среды сценариев Windows PowerShell.

> [!NOTE]
> Политика выполнения Windows PowerShell определяет, можно ли запускать сценарии и загружать профиль. Политика выполнения по умолчанию (Restricted) запрещает выполнение всех сценариев, включая профили. При использовании политики "Restricted" загрузить профиль нельзя. Дополнительные сведения о политике выполнения см. в статье [about_Execution_Policies [v4]](https://technet.microsoft.com/en-us/library/347708dc-1515-4d74-978b-8334603472e6).

## Выбор профиля для использования в интегрированной среде сценариев Windows PowerShell
Интегрированная среда сценариев Windows PowerShell поддерживает профили для текущего пользователя и для всех пользователей. Он также поддерживает профили Windows PowerShell, затрагивающие все узлы.

Выбор профиля зависит от того, каким образом вы используете Windows PowerShell и интегрированную среду сценариев Windows PowerShell.

-   Если для запуска Windows PowerShell используется только интегрированная среда сценариев Windows PowerShell, сохраните все элементы в одном из профилей интегрированной среды сценариев, таком как CurrentUserCurrentHost или AllUsersCurrentHost для интегрированной среды сценариев Windows PowerShell.

-   Если для запуска Windows PowerShell используется несколько основных программ, сохраните свои функции, псевдонимы, переменные и команды в профиле, затрагивающем все основные программы, таком как CurrentUserAllHosts или AllUsersAllHosts, и сохраните функции интегрированной среды сценариев, такие как настройки цветов и шрифтов, в CurrentUserCurrentHost или AllUsersCurrentHost для интегрированной среды сценариев Windows PowerShell.

Ниже указаны профили, которые можно создать и использовать в интегрированной среде сценариев Windows PowerShell. Каждый профиль сохраняется по собственному пути.

|Тип профиля|Путь к профилю|
|----------------|----------------|
|"Текущий пользователь, интегрированная среда сценариев PowerShell"|$profile.CurrentUserCurrentHost или $profile|
|"Все пользователи, интегрированная среда сценариев PowerShell"|$profile.AllUsersCurrentHost|
|"Текущий пользователь, все узлы"|$profile.CurrentUserAllHosts|
|"Все пользователи, все узлы"|$profile.AllUsersAllHosts|

## Создание профиля
Для создания профиля "Текущий пользователь, интегрированная среда сценариев PowerShell" выполните следующую команду:

```
if (!(test-path $profile )) 
{new-item -type file -path $profile -force}
```

Для создания профиля "Все пользователи, интегрированная среда сценариев PowerShell" выполните следующую команду:

```
if (!(test-path $profile.AllUsersCurrentHost)) 
{new-item -type file -path $profile.AllUsersCurrentHost -force}
```

Для создания профиля "Текущий пользователь, все узлы" выполните следующую команду:

```
if (!(test-path $profile.CurrentUserAllHosts)) 
{new-item -type file -path $profile.CurrentUserAllHosts -force}
```

Для создания профиля "Все пользователи, все узлы" введите следующее:

```
if (!(test-path $profile.AllUsersAllHosts)) 
{new-item -type file -path $profile.AllUsersAllHosts-force}
```

## Изменение профиля

1.  Чтобы открыть профиль, запустите команду psedit с переменной, которая указывает изменяемый профиль. Например, для открытия профиля "Текущий пользователь, интегрированная среда сценариев PowerShell" введите: `psEdit $profile`

2.  Добавьте несколько элементов в профиль. Ниже приведено несколько примеров, как можно приступить к работе:

    -   Чтобы изменить цвет фона по умолчанию для области консоли на синий, введите в файле профиля следующее: `$psISE.Options.OutputPaneBackground = 'blue'` . Дополнительные сведения о переменной $psISE см. в статье [Справочник по объектной модели интегрированной среды сценариев Windows PowerShell](https://technet.microsoft.com/en-us/library/e1a9e7d1-0fd5-47de-8d9b-f1be1ed13b0c).

    -   Чтобы изменить размер шрифта на 20, введите в файле профиля следующее: `$psISE.Options.FontSize =20`

3.  Чтобы сохранить файл профиля, в меню **Файл** щелкните **Сохранить**. Внесенные изменения применяются при следующем открытии интегрированной среды сценариев Windows PowerShell.

## См. также
[about_Profiles [v4]](https://technet.microsoft.com/en-us/library/e1d9e30a-70cc-4f36-949f-fc7cd96b4054)
[Использование интегрированной среды сценариев Windows PowerShell](Using-the-Windows-PowerShell-ISE.md)




<!--HONumber=Aug16_HO4-->


