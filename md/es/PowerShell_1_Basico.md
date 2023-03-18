---
title: PowerShell Básico
description: Introducción a PowerShell
permalink: /PowerShell_1_Basico/
---

# Introducción

***Windows Powershell*** es un programa basado en línea de comandos que aporta a los administradores de sistemas una herramienta perfecta para automatizar tareas. El tradicional entorno CMD se quedaba pequeño para administrar sistemas operativos cada vez más orientados a la red, de forma que Microsoft decidió renovarla con
Powershell.

Se ha diseñado para mejorar el entorno de línea de comandos y scripting mediante la eliminación de antiguos problemas y la incorporación de nuevas funciones.

Powershell no viene preinstalado en los sistemas anteriores a Windows 7, pudiendo instalarse como una actualización del sistema.

Características:
- Powershell hereda de su predecesora la mayoría de comandos relativos a carpetas: `mkdir`, `cd`, `dir`
- incluye una serie de comandos similares a Linux para mejorar la adaptación: `ls`, `clear`, `pwd`
- Usando la tecla TAB se pueden autocompletar comandos o rutas de archivos:
```
get-h [TAB]
Is c:\W [TAB]
```
si existen varias posibilidades un tabulador muestra la primera de ellas y posteriores pulsaciones del tabulador van mostrando las demás
- Pulsando las flechas del teclado se muestran los comandos ejecutados con anterioridad, pero si pulsamos
F7 se muestra una ventana donde podemos seleccionar los comandos anteriores.VENTAJAS POWERSHELL

## Ventajas de **PowerShell**

Windows PowerShell combina un entorno interactivo con un entorno de scripting que ofrece acceso a herramientas de línea de comandos y objetos COM (Componet Object Model), y permite aprovechar la eficacia de la biblioteca de clases de .NET Framework (FCL).

También mejora los scripts de Windows Script Host (WSH), que permiten utilizar varias herramientas de línea de comandos y objetos de automatización COM, pero que no proporcionan un entorno interactivo.

Aunque la interacción con Windows PowerShell se realiza mediante comandos de texto, Windows PowerShell está basado en objetos, no en texto. El resultado de un comando es un objeto.

## Formato de los comandos

Los comandos de `Powershell` se denominan cmdlets (commandlets).
Estos comandos tienen una sintaxis particular que se compone de un verbo, un sustantivo y después las opciones y argumentos:

    verbo-sust –parametro1 argumento1 -parametro2 argumento2 ...

Como se puede apreciar, el verbo y el sustantivo están separados por un guión(`-`) y los parámetros y argumentos por espacios.

Ejemplos:

```powershell
get-history             # Muestra una lista de los últimos comandos (por defecto 32)
get-history -count10    # Muestra los últimos 10 comandos ejecutados
get-history 7           # Muestra solamente el comando número 7 de la lista
get-history -id 7       # Muestra solamente el comando número 7 de la lista
```

### El concepto VERBO–SUSTANTIVO

Los **verbos** expresan acciones concretas en WPS.
- Get : obtiene
- Set : aplica
- Remove : elimina
- New : crea nuevo
- Clear : Limpia

Los **sustantivos** son muy parecidos a los de cualquier idioma, ya que describen tipos de objetos concretos que son importantes para la administración del sistema
Los sustantivos están menos limitados, pero deben describir siempre a qué se aplica un comando. 
- Command
- History
- ChildItem
- LocalUser
- LocalGroup
- NetAdapter
- Partition
- Host

Por ejemplo: 

```powershell
Get-Process, Stop-Process, Get-Service y Stop-Service   # Se aplican a procesos y servicios.
Get-LocalUser y Get-LocalGroup                          # Obtiene los usuarios y grupos locales del equipo
Get-Date                                                # Obtiene la fecha del sistema
Clear-Host                                              # Limpia la pantalla
Get-Location                                            # nos dice donde nos encontramos
Get-ChildItem                                           # Obtiene los elementos de la carpeta actual
Get-NetAdapter                                          # Tarjetas de red del sistema
```

Como vemos es muy complejo acordarse de todos los comandos, pero tenemos un sistema para intentar descubrir los comandos:
```
Get-Command –Verb Get                                   # Visualiza los cmdlets que usan el verbo Get
Get-Command -Noum LocalGroup                            # Todos los comandos que tienen algo de LocalGroup
Get-Commando *localgroup*                               # parecido al anterior, pero más amplio
Get-Command –noun Service                               # Visualiza cmdlets con sustantivo indicado
Get-Command -name Clear-Host                            # Muestra información de lo indicado a través del nombre, en este caso es una función.
```



## Ayudas en PowerShell

Es muy importante manejar el sistema de ayuda de Powershell. 

Para acceder a la ayuda disponemos de varios comandos y parámetros:

`get-help `ó `man` ó `help` ó `cmdlet -?` Todos acceden a la ayuda. `Man` y `Help` hacen paradas con `more`.

Opciones:

`-name cmdlet` Muestra ayuda de cmdlet indicado (puede omitir -name)
`-detailed `Muestra más detalle
`-full` Muestra la ayuda completa
`-examples `Muestra ejemplos de uso del cmdlet

Es aconsejable actualizar la ayuda de PowerShell con el comando `update-help`

Ejemplos:

```powershell
get-help                            # Muestra la ayuda del cmdlet get-help
get-help –detailed                  # Muestra más detalles del cmdlet get-help
get-help -full get-command          # Muestra la ayuda del comando get-command
get-help -examples get-command      # Muestra ejemplos de get-command
get-help about_*                    # Para mostrar la lista de temas conceptuales
get-help about_arithmetic_operators # Ayuda operadores aritméticos
```

Ejemplos con un comando concreto: `Get-Date`

```powershell
Get-Help Get-Date		    #ayuda de Get-Date
Get-Help Get-Date -Examples	#muestra ejemplos de uso
get-help Get-Date -detailed	#ayuda detallada
get-help Get-Date -full		#ayuda completa
get-help Get-Date -online	#ayuda online
get-help Write-Output -ShowWindow #Muestra la ayuda en una ventana
```

# Comando básicos de `PowerShell`

## `Get-Command` 
Muestra información básica sobre los cmdlets. Se suele utilizar para obtener listas de cmdlets.

Con `Get-Command` podemos obtener la lista de comandos, o por verbos, o por sustantivos
```powershell
Get-Command -Verb get     #de un verbo, los que sirven para obtener, modificar, etc...  
Get-Command -Noum Service #de un sustantivo, o sea los que tengan que ver con algo en concreto
Get-Command Clear-Host    #de un comando en concreto
```

## `Get-Member`

Todo lo que devuelve un cmdlet es un objeto. Un objeto es una forma de almacenar información, en la que se asocia a cada elemento una serie de datos denominados propiedades y una serie de acciones denominadas métodos.

Podemos apoyarnos en `Get-Member` para ver los objetos o métodos que componen un comando:

```powershell
Get-Command | Get-Member                # Todos los elementos de Get-Command
Get-Command | Get-Member -name Name     # lo que se llaman algo de Name

Get-Command | Where-Object{ $_.Name -like '*alias'} #Retorna
    # CommandType     Name                                               Version    Source
    # -----------     ----                                               -------    ------
    # Cmdlet          Export-Alias                                       3.1.0.0    Microsoft.PowerShell.Utility
    # Cmdlet          Get-Alias                                          3.1.0.0    Microsoft.PowerShell.Utility
    # Cmdlet          Import-Alias                                       3.1.0.0    Microsoft.PowerShell.Utility
    # Cmdlet          New-Alias                                          3.1.0.0    Microsoft.PowerShell.Utility
    # Cmdlet          Set-Alias                                          3.1.0.0    Microsoft.PowerShell.Utility
```


## Alias. Concepto y comandos.

Un **Alias** asocia un nombre de comando con otro comando. Se usan para evitar frustrar al usuario al usar un nuevo Shell. Hay de unix7linux como de ms-dos/cmd.

## `Get-Alias`

Obtiene los alias o un alias en concreto:

Sintaxis:
```
Get-Alias
   [[-Name] <String[]>]
   [-Exclude <String[]>]
   [-Scope <String>]
   [<CommonParameters>]
``` 
Muestra los alias:
```powershell
Get-Alias
Get-Alias -Name gp*, sp* -Exclude *ps
Get-Alias -Definition Get-ChildItem   # muestra todos los alias que utilizan Get-ChildItem
```

## `Set-Alias`

Crea o modifica un alias

Sintaxis:
```
Set-Alias
   [-Name] <string>
   [-Value] <string>
   [-Description <string>]
   [-Option <ScopedItemOptions>]
   [-PassThru]
   [-Scope <string>]
   [-Force]
   [-WhatIf]
   [-Confirm]
   [<CommonParameters>]
```

Ejemplos:

```PowerSHell
Set-Alias -Name list -Value Get-ChildItem
Set-Alias list Get-ChildItem                        #ídem anterior
Set-Alias -Name np -Value C:\Windows\notepad.exe    #Creando un alias a un ejecutable
 ``` 
Más información sobre Alias en PowerShell: [PowerShell: List of Aliases](http://xahlee.info/powershell/powershell_aliases.html)

## `Get-Date`

Para obtener la fecha. Ejemplos:

```powershell
Get-Date
Get-Date -UFormat %Y%m%d_%H%M%S	#Formato: 20200502_101213
Get-Date -UFormat "%d/%m/%Y %H:%M:%S"	#02/05/2020 09:52:24
Get-Date -UFormat "%A, %d de %B de /%Y %T %Z"	#sábado, 02 de mayo de /2020 09:55:29 +02


# Comandos para la gestión de Archivos y Carpetas

- `Get-Location` (pwd): Ruta o path actual

- `Set-Location` (cd): Para desplazarse.

- `Get-ChildItem` (ls): Muestra el contenido.

- `New-Item`  (ni/md): Para crear archivos y carpetas.

- `Remove-Item` (rm): Para Borrar archivos y carpetas.

- `Move-Item` (mv): Para mover archivos y carpetas.

- `Rename-Item` (ren): Para renombrar archivos y carpetas.

- `Copy-Item` (cp): Para copiar archivos y carpetas.

- `Get-Content` (cat): Para mostrar el contenido de un fichero



Ejemplos:
```ps
Get-ChildItem c:\ -Attributes hidden    # Muestra los archivos ocultos de c:\
New-Item Prueba1 -itemType Directory    # Crea una carpeta
New-Item .\Prueba1\texto1.txt           # Crea un fichero dentro de la carpeta anterior
Remove-Item .\Prueba1\ -recurse         # Elimina el directory
New-Item Foto1.jpg,Foto2.jpg
New-Item Fotos -ItemType Directory
Move-Item *.jpg .\Fotos\                # Mueve todos los archivos .jpg a la carpeta Fotos
Rename-Item .\Fotos\ Recuerdos                  # Renombra una carpeta
Copy-Item .\Recuerdos\ CopiaRecuerdos -recurse  # Copia en una nueva carpeta la carpeta existente Recuerdos
Get-Date > prueba.txt                   # Crea un nuevo fichero que contiene la información de la fecha
Get-Content .\prueba.txt                # Muestra el contenido del fichero anterior
```


# Tuberias y redirecciones

El mecanismo de tubería conocido en los sistemas Linux funciona de igual forma en Powershell, pero su uso es aun más intensivo, ya que la información que muestran los comandos, el formato de salida, etc, se controla usando este mecanismo.

En el siguiente enlace de [Profesional Review: Redirecciones y Tuberias en línux](https://www.profesionalreview.com/2017/02/19/redirecciones-tuberias-linux/) tienes más información

Ejemplo:

```powershell
ls c:\windows | more # Tubería para realizar paradas de la salida del comando ls
ls c:\windows >fichero.txt # Redireccionamiento de salida para guardar en el archivo indicado la lista de archivos y directorios del subdirectorio Windows.
ls c:\windows >>fichero.txt # Lo mismo que el anterior pero añadiendo la información al final del archivo indicado.
```

Para ver el contenido de estos ficheros podemos usar los alias cat o type.
- El alias `ls` corresponde al cmdlet `get-ChildItem `
- `cat` ó `type` a `Get-Content`


# Formateando y ordenando la salida
## `Format-List`

Forma en un listado 
```
Format-List
      [[-Property] <Object[]>]
      [-GroupBy <Object>]
      [-View <string>]
      [-ShowError][-DisplayError]
      [-Force]
      [-Expand <string>]
      [-InputObject <psobject>]
      [<CommonParameters>]
```
```PowerShell
Get-Process -name PowerShell | Format-List
Get-Process | Format-List -Property id, name            # Muestra solo las propiedades concretas
```

## `Format-Wide`

Formatea en columnas, las que indiquemos o quepan
```
Format-Wide
      [[-Property] <Object>]
      [-AutoSize]
      [-Column <int>]
      [-GroupBy <Object>]
      [-View <string>]
       [-ShowError]
      [-DisplayError]
      [-Force]
      [-Expand <string>]
      [-InputObject <psobject>] 
      [<CommonParameters>]
```
```PowerShell
Get-ChildItem | Format-wide -column 3                   # muestra el listado en 3 columnas
```

## `Format-Table`

Formatear en una especie de tabla
```
Format-Table
      [-AutoSize]
      [-RepeatHeader]
      [-HideTableHeaders]
      [-Wrap]
      [[-Property] <Object[]>]
      [-GroupBy <Object>]
      [-View <String>]
      [-ShowError]
      [-DisplayError]
      [-Force]
      [-Expand <String>]
      [-InputObject <PSObject>]
      [<CommonParameters>]
```

```powershell
Get-Process | Format-Table -property id, ProcessName
Get-Process | format-Table -groupby ProcessName -property id, ProcessName # Además agrupa
```

## `Sort-Object`

Ordena el listado por una o varias propiedades. 

```powershell
Get-Process | Sort-Object -property id -desc
Get-Process | Sort-Object -property id | Format-Table -property id, ProcessName
```

Recordad que si deseamos conocer todas las propiedades de un comando, tenemos el cmdlet `Get-Member` que nos muestra todos los objetos que forman parte del comando y mediante `-MemberType Property` filtramos únicamente todas las propiedades del comando.

En este caso, vamos a utilizar el comando a traves de una *pipe* o tub*ería.

```powershell
ls | Get-Member -MemberType Property	# muestra las propiedades del comando.
```

## `Where-Object`

Permite realizar un filtro mediante una condición concreta entre los resultados de un comando determinado. Siempre se utiliza después de una tubería

Ejemplo: 
```powershell
Get-Service                                                     # Muestra todos los servicios del sistema
Get-Service | Where-Object { $_.Status -like 'Running'}         # Servicio con estado *Running*
Get-Process | Where-Object { $_.PriorityClass -eq "Normal"}     # Servicios con prioridad Normal
```

- **Notas**: 
  - `$_` representa el conjunto total de elementos como resultado de la ejecución del comando anteior a la tuberia
  - `&_.` el punto (`.`) se utiliza para especificar una propiedad de todas las obtenidas entre los resultados del comando 


Más ejemplos: 
```powershell
Get-ChildItem | Where-Object {$_.FullName -match '[a-z]+\.*txt$'}                                       # donde el nombre "algo" y luego termine por .txt
Get-ChildItem | Where-Object {$_.Length -ge 1000 -and $_.LastAccessTime -gt (Get-Date '2020/03/30')}    # listar archivos que sean de mas de 1000 kb y que el último acceso sea posterior a una fecha
```

## `Select-Object`

Seleccionar objetos que vienen de la tubería. 

```powershell
Get-ChildItem c:\ | Select-Object -first 5 -unique
Get-ChildItem c:\ | Select-Object -last 5 -unique
Get-Content .\fich1.txt | Select-Object -Last 3 # Seleccionar las 3 últimas líneas de un fichero
```
En el caso anterior, selecciona todas las propiedades de los objetos indicados (los 5 primeros o los 5 últimos) , pero también selecciona unas propiedades específicas entre todos los resultados obtenidos del comando:

```powershell
Get-Process | Select-Object -Property ProcessName, Id, WS                   # Obtiene 3 propiedades concretas
Get-Process Notepad  | Select-Object id, ProcessName, StartTime -First 3    # Seleccionar 3 propiedades pero solo los 3 primeros registros
```







# CmdLista para la gestión de SERVICIOS

## `Get-Service`
Obtiene el listado de servicios del equipo:

```powershell
Get-Service -Displayname "*network*"
Get-Service -Name "win*" -Exclude "WinRM"
Get-Service | Where-Object {$_.DisplayName -like "*hora*"} | Select-Object Name
Get-Service | Where-Object {$_.DisplayName -like "*time*" -or $_.DisplayName-like "*hora*"} | Select-Object Name
gsv -Name a* # alias
gsv -displayname "*hora*"
```

- Servicios que tengan cuyo nombre comience por una vocal (manuscula o minuscula)
```powershell
Get-Service | Where-Object { $_.name -match '^[aeiou]'}
```
- Búsqueda de servicios dependientes
```powershell
Get-Service s* | Select-Object name, DependentServices
```

- Sobre un servicio en concreto, expandir las propiedades 
```powershell
Get-Service SstpSvc | Select-Object name, DependentServices | select -ExpandProperty dependentServices
```

## `Start-Service`
- Activando un servicio
```powershell
gsv -displayname "*hora*"
Start-Service W32Time
```

- Todos los servicios de host que están funcionando
```powershell
Get-Service | where {$_.DisplayName -like "*host*"} | Where-Object {$_.Status -like "running"}
```

- Más ejemplo
```powershell
Get-Service s* | Select-Object name, RequiredServices, DependentServices
```

# cmdLets para la gestión de PROCESOS.

## `Get-Process`

Listar procesos
```powershell
Get-Process -name se*
```

## `Start-Process`

Iniciar un proceso
```powershell
Start-Process notepad
```

## `Stop-Process`

Eliminar un proceso por el n?mero de id
```powershell
Stop-Process -id 8208
```

**Ejemplo**: Eliminar un proceso. Por varios metodo, pero por ejemplo, por su nombre
```powershell
Get-Process -name "notepad" | Stop-Process
Stop-Process -ProcessName notepad

# En este caso elimina utilizando la propia función de eliminar el objeto
Get-Process -name "notepad" | Where-Object {$_.kill()}
```

### Para ver si tenemos el método que mata **(kill)**: Si lo ejecutas sale el listado, y tambien puedes ver las propiedades, etc...
```powershell
Get-Process Notepad | Get-Member -MemberType method

# Confirmación antes de matar con --> **-Confirm**
Stop-Process -name "Notepad" -Confirm

# Módulos que utiliza un programa --> **-module**
Get-Process -name "Notepad" -module

# Versión del programa --> **-FileVersionInfo**
Get-Process Notepad -FileVersionInfo

#aui utilizamos el alias ps
ps -name s* | select id, ProcessName, StartInfo -first 5 
```


### `Format-List`
Mostrando procesos en forma de lista
```powershell
Get-Process s* | Select-Object id, ProcessName | format-list
```

### `Sort-Object`
Seleccionando solo *"ProcessName"* , valores únicos y ordenarlo pero por el *id*
```powershell
Get-Process s* | Select-Object ProcessName -uniq | Sort-Object -Property id
```

**Ejemplo:** mostrar por orden de uso desciende de CPU
```powershell
Get-Process s* | Select-Object id, ProcessName, CPU | Sort-Object -Property cpu -Descending 
ps -name s* | sort cpu -desc | select id, ProcessName, cpu -first 5 #solo 5
```

# FILTROS: 

## `Where-Object`
Filtrado de contenidos mediante mecanismo de tubería más `Where-Object` 
Utilizan
- `-eq`, `-ne`, `-gt`, `-ge`, `-lt`, `-le`
- `-like` : Comparaciones con comodines.
```
powershell -like 'power'
powershell -like 'power*'
powershell -like 'po?er*'
```
- [`-match`](https://docs.microsoft.com/es-es/powershell/module/microsoft.powershell.core/about/about_regular_expressions?view=powershell-7.1)
```powershell
'coordinar' -match 'oo'
'coordinar' -match 'o'
'coordinar' -match 'oooo'
'Fin' -match 'F[iou]n'
'Fan' -match 'F[iou]n'
'Fin' -match 'F[^iou]n'
42 -match '[0-9][0-9]'     # filtramos que solo san dos número
'4X' -match '[0-9][0-9]'
'4X' -match '[0-9][A-Z]'   # Validando también alfanuméricos: debe ser una cadena lo que evaluemos
'A3' -match '[A-Z][A-Z]'   # da falso
'pc-01' -match 'pc-\d\d'   # Filtra que haya dos dígitos decimales al final     
'palabra' -match '\w'      # da verdadero porque \w indica una palabra
'Fich1' -match '.....'     # Cada . representa un punto: si hay más caracteres que puntos también es correcto
'Fich1' -match '.[aeiou]'  # True porque encuentra alguna vocal después de un carácter
'Fch1' -match '.[aeiou]'   # False porque no encuentra alguna vocal después de un carácter
'aFch1' -match '^.[aeiou]'     # False porque no encuentra al principio un carácter seguido de una vocal
'pce-01' -match '[A-Z]*-\d\d'   # Empieza por una mayúscula y tiene algo , una guión y dos números
"-5" -match "^[+-]?[0-9]+" #para un número que puede comenzar por + y por menos
"correo@correo.com" -match "[a-zA-Z0-9_%+-.]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6}$" # para correos electrónicos
```

- etc...

**Ejemplos** con `Where-Object`

```powershell
# Listando procesos cuyo nombre tiene menos de 4 letras
Get-Process | Where-Object {$_.ProcessName -notmatch '....'}

# Procesos o ficheros que ocupan mas de XXX
Get-Process | Where-Object {$_.WorkingSet -gt 200000000}
ls c:\windows | Where-Object{ $_.length -gt 1000000}

# Mostrar los fichero modificados a partir del uno de enero de 2021
ls c:\windows | Where-Object{ $_.LastWriteTime -gt (get-date "2021-01-01")}
ls c:\windows | Where-Object{ ($_.LastWriteTime -gt (get-date "2021-01-01")) -and ($_.length -le 5000)}
ls c:\windows | ?{ ($_.LastWriteTime -gt (get-date "2021-01-01")) -and ($_.length -le 5000)}

# Con operadores lógicos negados y expresiones regulares negadas
Get-ChildItem c:\windows | ?{($_.FullName -notlike "*.exe")}

# muestra los fichero con atributos reanonly
Get-ChildItem c:\windows | ?{$_.Attributes -match "readonly"} 

# responding --> muestra los procesos que NO responden 
Get-Process | Where-Object {-not $_.responding}

# Idem anterior, pero nos lo cargamos
Get-Process | Where-Object {-not $_.responding} | Stop-Process
```

Se aplica sobre objetos y devuelve objetos.
```powershell
# Ejemplos básicos
2000, 3000, 4000 | ForEach-Object { $_}
1..5 | ForEach-Object { $_}
1..5 | % { $_}  # usando el alias que es %

# Ejemplo listar procesos
Get-Process | ForEach-Object {$_.name}
Get-Process | ForEach-Object {$_.name} | Select-Object -unique  # filtra por únicos
Get-ChildItem | ForEach-Object{$_.Name}

# Listamos concatenando información.
Get-ChildItem | ForEach-Object {$_.name + " ---> "  + $_.Length}
```

- **Ejemplo**: Una tabla de multiplicar ...
```powershell
0..10 | ForEach-Object{ "2 x " + $_ + " = " + $_*2}
```

- **Ejemplo**: Contamos ficheros, 
```powershell
Get-ChildItem | ForEach-Object {$i++};echo $i
$i=0; Get-Service | ForEach-Object{ $i++}; echo $i

# Cogemos los ficheros de mas de 1024 kb de un directorio y pasamos el tamaño a kb
Get-ChildItem C:\Windows\ | Where-Object { $_.Length -gt 1024} | ForEach-Object {$_.Length/1024}

# Utilizamos una función de un fichero, para hacer una copia uno a uno de un directorio a otro
Get-ChildItem | ForEach-Object {$_.CopyTo('c:\temp\'+$_.Name)}
Get-ChildItem | % {$_.CopyTo('c:\temp\'+$_.Name)} # % es el alias de forEach-Object

# Idem anterior, pero para borrar
Get-ChildItem C:\temp\*.md | ForEach-Object {$_.Delete()}

# lo mismo, sin usar el m?todo y usando un cmdlet
Get-ChildItem C:\temp\*.md | ForEach-Object { renove-item $_.FullName}

# Copiar fichero, cambiando la extensión y usando funciones
Get-ChildItem *.txt | ForEach-Object {$_.CopyTo($pwd.Path+"\"+$_.BaseName+".texto")}
```

# OBJETOS **WMI**

**WMI**: *Windows Management Instrumentation*. Esta previamente instalado y es una herramienta que sirve para gestionar los elelemtos Hardware de nuestro sistema a través de PowerShell

Por ejemplo, podemo obtener información del sistema
```powershell
Get-WmiObject -class win32_processor	#Muestra informaicón del procesador
Get-WmiObject -class win32_logicaldisk	#discos lógios
Get-WmiObject -class win32_BIOS         # infomración de la BIOS
Get-WmiObject -list 		#lista todos los elementos posibles
```

# WMIC

A diferencia de `WMI` lo hace todo con el comando `get-WmiObject` con `WMIC` tenemos especie de extensión que se obtiene con el comando `wmic`. Una de las diferencias que con `wmic` debemos especificar las salidas.
- wmic opciones_globales alias formatoDeSalida

```powershell
wmic /output:stdout bios get /all /format:list #información del a BIos
wmic /output:stdout CPU get /all /format:list	#inforamción de la CPU
wmic /output:cpu.txt CPU get /all /format:list	#inforamción de la CPU
wmic /output:stdout nicconfig get /all /format:list	#inforamción de las tarjetas de red
wmic /output:stdout diskdrive get /all /format:list	#información de los discos físicos
wmic /output:stdout logicaldrive get /all /format:list #información de los discos físicos
wmic /output:stdout recoveros get /all /format:list #información sobre los errores del sistema operativo
```

-  Creación de objetos con ***wmic***
```powershell
wmic process call create "Notepad.exe" # ejecuta el notepad
```

# REGISTRO de windows

Desde `PowerShell` podemos acceder al registro de Windows únicamente para ***consulta***.

Veamos ejemplos de acceso a las opciones de desinstalado, y podemos iterar sobre ellas como con todos los objetos 
```powershell
Get-ChildItem Registry::HKLM\Software\Microsoft\Windows\Currentversion\Uninstall
ls Registry::HKLM\Software\Microsoft\Windows\Currentversion\Uninstall #usando alias
Get-ChildItem Registry::HKLM\Software\Microsoft\Windows\Currentversion\Uninstall\w* | foreach {$_.getvaluenames()}
Get-ChildItem Registry::HKLM\Software\Microsoft\Windows\Currentversion\Uninstall\w* | foreach {$_.getvalue('versionmajor')}
Get-ChildItem Registry::HKLM\Software\Microsoft\Windows\Currentversion\Uninstall\w* | foreach {$_.getvalue('displayversion')}
```