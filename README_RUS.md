![top-image](/p0h-K9dEJMs.jpg)

// Пометка: учитывая, что у меня нет Office 365 и возможности до него добраться в течение выделенных часа-двух на текущее задание, скриншоты прикрепить не смогу. 

### Contents:
* [Добавление адресов Atmosphere в белый список]()
  * [Требования]()
  * [Добавление IP-адресов через Defender Portal]()
  * [Добавление IP-адресов через PowerShell]()
  * [Проверьте результаты]()

# Добавление адресов Atmosphere в белый список
Для того, чтобы письма Atmosphere беспрепятственно доходили до адресата, необходимо добавить IP-адреса в настройки Office 365.

> Необходимо иметь в виду, что инструкция подходит только для пакетов Office 365 или Microsoft 365. Ранние версии вроде Exchange 2013 или Exchange 2016 предполагают другие шаги.

Возможно добавить IP-адреса Atmosphere двумя путями:
* Через веб-интерфейс (Microsoft Defender Portal  365).
* Через PowerShell.

## Требования
* У вас должны быть необходимые разрешения в Exchange Online:
	* Organization management.
	* Security administrator role.

## Добавление IP-адресов через Defender Portal
1. Перейти на портал и пройти **Email & Collaboration** > **Policies & Rules** > **Threat policies** > секция **Policies** > **Anti-spam**.
2. На странице **Anti-spam policies**, выберите **Connection filter policy (Default)** из списка, нажав н название политики.
3. В появившемся окне можно редактировать следующие поля:
	* **Description**: Поле для ввода описательного текста. После ввода необходимо нажать на **Save**.
4. **Connection filtering section**: Click **Edit connection filter policy**. In the flyout that appears, configure the following settings:
	* **Always allow messages from the following IP addresses or address range**: Это белый список IP-адресов. Click in the box, enter a value, and then press Enter or select the complete value that's displayed below the box: `‌5.188.156.32` Repeat this step for the second IP address `139.99.122.10`. Необходимо нажать на поле, ввести значение, и  нажать Enter/отметить предлагаемый вариант под полем. Чтобы убрать существующее значение, необходимо нажать на иконку **Remove** рядом со значением.
5. Всё. Для сохранения, нажать **Save**.


## Добавление IP-адресов через PowerShell
Использовать следующие команды в Shell
* Для того, чтобы переписать существующие значения:
	* `Set-HostedConnectionFilterPolicy -Identity Default -IPAllowList 5.188.156.32, 139.99.122.10`
* Для добавления новых значений:
	* `Set-HostedConnectionFilterPolicy -Identity Default -IPAllowList @{Add="5.188.156.32, 139.99.122.10"}`
* Для удаления существующих значений (в случае ошибки):
	* `Set-HostedConnectionFilterPolicy -Identity Default -IPAllowList @{Remove="[здесь_удаляемый_ip]"}`


## Проверьте результаты
Если вам необходимо проверить результаты, то:
* На Microsoft 365 Defender portal, пройти: **Email & Collaboration** > **Policies & Rules** > **Threat policies** > **Policies section** > **Anti-spam** > выбрать **Connection filter policy (Default)** из списка при нажатии на название политики, и проверить наличие IP-адресов в настройках.
* В Exchange Online PowerShell или EOP PowerShell, запустить команду и проверить настройки: `Get-HostedConnectionFilterPolicy -Identity Default`
* Send a test message from an entry on the IP Allow List.
* Отправить тестовое сообщение с указанных IP-адресов: `5.188.156.32, 139.99.122.10`
