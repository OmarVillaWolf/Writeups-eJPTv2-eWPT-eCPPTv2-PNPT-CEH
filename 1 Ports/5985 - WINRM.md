# WINRM ❮❯

[Windows Remote Management](https://docs.microsoft.com/en-us/windows/win32/winrm/portal) (`WinRM`) is the Microsoft implementation of the network protocol [Web Services Management Protocol](https://docs.microsoft.com/en-us/windows/win32/winrm/ws-management-protocol) (`WS-Management`). It is a network protocol based on XML web services using the [Simple Object Access Protocol](https://docs.microsoft.com/en-us/windows/win32/winrm/windows-remote-management-glossary) (`SOAP`) used for remote management of Windows systems. It takes care of the communication between [Web-Based Enterprise Management](https://en.wikipedia.org/wiki/Web-Based_Enterprise_Management) (`WBEM`) and the [Windows Management Instrumentation](https://docs.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page) (`WMI`), which can call the [Distributed Component Object Model](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-dcom/4a893f3d-bd29-48cd-9f43-d9777a4415b0) (`DCOM`).

Cuando tengamos credenciales validas en el crackmapexec, podemos usar este servicio para conectarnos.
- Comando -> **gem install evil-winrm** Instalamos el evil-winrm para podernos conectar al servicion de winrm

❯ **evil-winrm -i ❮IP_Target❯ -u ❮Username❯ -p ❮Passwd❯** Con este comando nos conecta al servicio de winrm y podemos entrar a la maquina


