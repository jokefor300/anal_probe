![top-image](/p0h-K9dEJMs.jpg)

// Пометка: учитывая, что у меня нет Office 365 и возможности до него добраться в течение выделенных часа-двух на текущее задание, скриншоты прикрепить не смогу. 

### Contents:
* [Add Atmosphere to your white list]()
  * [Prerequisites]()
  * [Add IP addresses to the white list through web interface (Defender Portal)]()
  * [Add IP addresses to the white list through PowerShell]()
  * [Check your work]()

# Add Atmosphere to your white list

To ensure that Atmosphere emails reach you easily, you need to add our IPs to your Office 365 settings.

> Note that this instruction is valid only for Office 365 or Microsoft 365 releases (the steps for Exchange 2013 and 2016 are different).

You can add chosen IPs in two ways:
* Web interface (Microsoft Defender Portal 365).
* PowerShell.

## Prerequisites
* You have to have necessary Exchange Online permissions:
	* Organization management or Security administrator role.
 
## Add IP addresses to the white list through web interface (Defender Portal)

1. In the Microsoft 365 Defender portal, go to **Email & Collaboration** > **Policies & Rules** > **Threat policies** > **Policies** section > **Anti-spam**.
2. On the **Anti-spam policies** page, select **Connection filter policy (Default)** from the list by clicking on the name of the policy.
3. In the policy details flyout that appears, configure any of the following settings:
	* **Description** section: Click **Edit name and description**. In the Edit name and description flyout that appears, enter optional descriptive text in the **Description** box.
		When you're finished, click **Save**.
4. **Connection filtering section**: Click **Edit connection filter policy**. In the flyout that appears, configure the following settings:
	* **Always allow messages from the following IP addresses or address range**: This is the IP Allow list. Click in the box, enter a value, and then press Enter or select the complete value that's displayed below the box: `‌5.188.156.32` Repeat this step for the second IP address `139.99.122.10`. To remove an existing value, click remove Remove icon next to the value.
5. Finish. Click **Save**.

## Add IP addresses to the white list through PowerShell
Use the following command in the shell:
* To overwrite:
	* `Set-HostedConnectionFilterPolicy -Identity Default -IPAllowList 5.188.156.32, 139.99.122.10`
* To add:
	* `Set-HostedConnectionFilterPolicy -Identity Default -IPAllowList @{Add="5.188.156.32, 139.99.122.10"}`
* To remove a specific address (in case of mistake):
	* `Set-HostedConnectionFilterPolicy -Identity Default -IPAllowList @{Remove="5.188.156.32, 139.99.122.10"}`


## Check your work
If you need to assure that new changes are in place, you can:
* In the Microsoft 365 Defender portal, go to **Email & Collaboration** > **Policies & Rules** > **Threat policies** > **Policies section** > **Anti-spam** > select **Connection filter policy (Default)** from the list by clicking on the name of the policy, and verify the settings.
* In Exchange Online PowerShell or standalone EOP PowerShell, run the following command and verify the settings: `Get-HostedConnectionFilterPolicy -Identity Default`
* Send a test message from an entry on the IP Allow List.
