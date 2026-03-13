1. Instalace IIS a CGI
Nejdříve musíme zprovoznit samotný webový server a povolit mu komunikaci s PHP.

Otevřete Server Manager -> Add roles and features.

Proklikejte se k Server Roles a zaškrtněte Web Server (IIS).

V podnabídce (Features) pod Web Server -> Application Development musíte nutně zaškrtnout CGI. Bez toho IIS nespustí PHP.

Dokončete instalaci.

2. Instalace PHP pro Windows
IIS potřebuje PHP v "Non-Thread Safe" (NTS) verzi.

Stáhněte si PHP (aktuální verzi, např. 8.2 nebo 8.3) z windows.php.net. Vyberte VS16 x64 Non-Thread Safe.

Obsah zipu rozbalte do složky, např. C:\php.

Ve složce najděte php.ini-production, zkopírujte ho a přejmenujte na php.ini.

V php.ini odkomentujte (smažte středník) a nastavte tyto důležité direktivy:

extension_dir = "ext"

cgi.force_redirect = 0

cgi.fix_pathinfo = 1

A povolte potřebné extenze: curl, fileinfo, gd, mbstring, openssl, pdo_mysql (nebo jinou DB).

Důležité: Nainstalujte Visual C++ Redistributable, jinak PHP nepoběží.

3. Instalace URL Rewrite modulu
Laravel spoléhá na hezké URL (routování). IIS standardně nerozumí souborům .htaccess. Potřebujete doplněk.

Stáhněte a nainstalujte Rewrite Module for IIS. Bez tohoto kroku vám pojede jen úvodní stránka a ostatní vyhodí chybu 404.

4. Konfigurace IIS pro PHP
Nyní musíme IIS říct, že soubory .php má předávat našemu PHP v C:\php.

Otevřete IIS Manager.

Klikněte na název serveru a otevřete Handler Mappings.

Vpravo klikněte na Add Module Mapping.

Request path: *.php

Module: FastCgiModule

Executable: C:\php\php-cgi.exe

Name: PHP_via_FastCGI

Potvrďte a nechte IIS vytvořit "FastCGI application".

5. Nasazení projektu a práva
Zkopírujte svůj Laravel projekt z Dockeru (vaše zdrojové kódy) do složky na serveru, např. C:\inetpub\wwwroot\muj-projekt.

Práva ke složkám (Klíčový krok)
IIS uživatel musí mít právo zapisovat do logů a cache.

Klikněte pravým na složku storage a bootstrap/cache.

V záložce Security přidejte uživatele IIS_IUSRS.

Nastavte mu Full Control (nebo alespoň Write/Modify).

6. Vytvoření Web.config
Protože IIS nepoužívá .htaccess, musíte v rootu složky public (ne v rootu projektu!) vytvořit soubor web.config.

Vložte do něj tento standardní kód pro Laravel:

XML
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <rewrite>
            <rules>
                <rule name="Laravel Force public" stopProcessing="true">
                    <match url="(.*)" ignoreCase="false" />
                    <action type="Rewrite" url="index.php/{R:1}" />
                </rule>
            </rules>
        </rewrite>
    </system.webServer>
</configuration>
7. Finální nastavení webu v IIS
V IIS Manageru klikněte pravým na Sites -> Add Website.

Physical Path: Musí směřovat do složky public vašeho projektu (nikoliv do kořenové složky projektu).

Nastavte si Hostname nebo port.

.env a Composer
Nezapomeňte na serveru v kořeni projektu upravit soubor .env (připojení k DB, která už pravděpodobně neběží v Dockeru, ale na Windows nebo v cloudu).

Pokud nemáte na serveru Composer, nainstalujte ho a spusťte v projektu composer install --no-dev.

Tip z praxe: Pokud se vám po spuštění zobrazí bílá stránka nebo chyba 500, zapněte si v .env dočasně APP_DEBUG=true. Pokud je chyba v IIS (ještě před PHP), podívejte se do Event Viewer -> Windows Logs -> Application.

Chtěl byste pomoci s nastavením konkrétní databáze (MySQL/SQL Server) na tom samém stroji, nebo už databázi máte vyřešenou?