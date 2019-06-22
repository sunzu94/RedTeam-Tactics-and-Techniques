# Word Library Add-Ins

It' possible to persist in the userland by abusing word library add-ins by putting your malicious DLL into a Word's trusted location. Once the DLL is there, the Word will load it next time it is opened.

## Execution

Get trusted locations:

{% code-tabs %}
{% code-tabs-item title="attacker@victim" %}
```csharp
 Get-ChildItem "hkcu:\Software\Microsoft\Office\16.0\Word\Security\Trusted Locations"
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![](../../.gitbook/assets/annotation-2019-06-22-121402.png)

Which are actually defined here if you access to the GUI:

![](../../.gitbook/assets/annotation-2019-06-22-121426.png)

Let's create a simple DLL that will launch a notepad.exe:

![](../../.gitbook/assets/annotation-2019-06-22-143558.png)

Copy over the malicious DLL into a `Startup` folder and rename it to `evilm64.wll`:

![](../../.gitbook/assets/annotation-2019-06-22-121537.png)

```text
mv .\evilm64.dll .\evilm64.wll
```

![](../../.gitbook/assets/annotation-2019-06-22-144024%20%281%29.png)

Next time the victim opens up the Word, evilm64.dll will be loaded and executed:

![](../../.gitbook/assets/annotation-2019-06-22-143432.png)

Interesting to note that procexplorer does not see the evilm64.wll loaded:

![](../../.gitbook/assets/annotation-2019-06-22-144128.png)

although we can definitely see that the add-in is now recognized by Word:

![](../../.gitbook/assets/annotation-2019-06-22-144219.png)

{% hint style="info" %}
This technique did not work for me on Office 365 version, but worked on Office Professional. Not sure if there's a bug in the 365 version or it's just a limitation of that version.
{% endhint %}

## References

{% embed url="https://www.mdsec.co.uk/2019/05/persistence-the-continued-or-prolonged-existence-of-something-part-1-microsoft-office/" %}

{% embed url="https://labs.mwrinfosecurity.com/blog/add-in-opportunities-for-office-persistence/" %}
