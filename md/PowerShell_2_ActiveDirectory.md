---
title: PowerShell AD
permalink: /PowerShell_2_ActiveDirectory/
---


- [1. Módulos en PowerShell](#1-módulos-en-powershell)
- [2. PowerShell para administración de *Active Directory*](#2-powershell-para-administración-de-active-directory)
  - [2.1. Comandos para la gestión de **usuarios**](#21-comandos-para-la-gestión-de-usuarios)
    - [2.1.1. `New-ADUser`](#211-new-aduser)
    - [2.1.2. `Remove-ADuser`](#212-remove-aduser)
    - [2.1.3. `Get-ADUser`](#213-get-aduser)
    - [2.1.4. `Set-ADUser`](#214-set-aduser)
    - [2.1.5. `Move-ADUser`](#215-move-aduser)
    - [2.1.6. `Enable-ADAccount` y `Disable-ADAccount`](#216-enable-adaccount-y-disable-adaccount)
  - [2.2. Comandos para la gestión de **grupos** de usuarios](#22-comandos-para-la-gestión-de-grupos-de-usuarios)
    - [2.2.1. `New-ADGroup`:](#221-new-adgroup)
    - [2.2.2. `Remove-ADGroup`:](#222-remove-adgroup)
    - [2.2.3. `Add-ADGRoupMember`](#223-add-adgroupmember)
    - [2.2.4. `Remove-ADGroupMember`](#224-remove-adgroupmember)
    - [2.2.5. `Get-ADGroupMember`](#225-get-adgroupmember)
    - [2.2.6. `Get-AdPrincipalGroupMembemship`](#226-get-adprincipalgroupmembemship)
  - [2.3. Comandos para la gestión de **Unidades Organizativas**](#23-comandos-para-la-gestión-de-unidades-organizativas)
    - [2.3.1. `New-ADOrganizationalUnit`](#231-new-adorganizationalunit)
    - [2.3.2. `get-ADOrganizationalUnit`](#232-get-adorganizationalunit)
    - [2.3.3. `Remove-AdOrganizationaUnit`](#233-remove-adorganizationaunit)
  - [2.4. Comando para la gestión de equipos](#24-comando-para-la-gestión-de-equipos)
    - [2.4.1. `New-ADComputer`](#241-new-adcomputer)
    - [2.4.2. `Set-ADComputer`](#242-set-adcomputer)
    - [2.4.3. `Get-ADComputer`](#243-get-adcomputer)
    - [2.4.4. `Remove-ADComputer`](#244-remove-adcomputer)
  - [2.5. Otros comandos de Active Directory](#25-otros-comandos-de-active-directory)
    - [2.5.1. `get-ADObject`](#251-get-adobject)
    - [2.5.2. `Move-ADObject`](#252-move-adobject)
    - [2.5.3. `Get-ADForest`](#253-get-adforest)
    - [2.5.4. `Get-ADDomainCotroller`](#254-get-addomaincotroller)
    - [2.5.5. `search-ADAccount`](#255-search-adaccount)
    - [2.5.6. `Set-ADAccountExpiriation` y `Clear-ADAccountExpiration`](#256-set-adaccountexpiriation-y-clear-adaccountexpiration)
    - [2.5.7. `Unlock-ADAccount`](#257-unlock-adaccount)
    - [2.5.8. `Set-ADAccountPassword`](#258-set-adaccountpassword)
  - [2.6. Tabla resumen de comandos y sufijos](#26-tabla-resumen-de-comandos-y-sufijos)
  - [2.7. Filtros: Ejemplos de uso con comandos de Active Directory](#27-filtros-ejemplos-de-uso-con-comandos-de-active-directory)
- [3. Active Directory - Permisos](#3-active-directory---permisos)
  - [3.1. Gestión de permisos SMB](#31-gestión-de-permisos-smb)
    - [3.1.1. `New-SmbShare`](#311-new-smbshare)
    - [3.1.2. Sufijo `SmbShare`](#312-sufijo-smbshare)
  - [3.2. Gestión de Permisos NTFS](#32-gestión-de-permisos-ntfs)
    - [3.2.1. `Get-Acl` .Ver los permisos de una carpeta](#321-get-acl-ver-los-permisos-de-una-carpeta)
    - [3.2.2. Deshabilitar la herencia](#322-deshabilitar-la-herencia)
    - [3.2.3. Eliminar todos los permisos de un usuario](#323-eliminar-todos-los-permisos-de-un-usuario)
    - [3.2.4. Añadir permisos a un usuario](#324-añadir-permisos-a-un-usuario)
    - [3.2.5. Funcion **```New-Object System.Security.AccessControl.FileSystemAccessRule()```**](#325-funcion-new-object-systemsecurityaccesscontrolfilesystemaccessrule)


# 1. Módulos en PowerShell

Son paquetes que contienen comandos especificos para la administración de una faceta del sistema.

- `Get-Module` para listar los módulos que instalados en el sistema

```ps
Get-Module
Get-Module -ListAvailable # Lista los módulos cargados o disponibles pero no instalados
```

[Referencia de Microsoft PowerShell: Get modules installed on a computer](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/get-module?view=powershell-7.1#example-9--get-modules-installed-on-a-computer)


Si queremos instalar un módulo nuevo porque necesitamos gestionar un caractarística o rol en concreto desde PowerShell, entonces debemos instalar el módulo mendiante el comando `Import-Module`

- `Import-module` para instalar un nuevo módulo

```powershell 
import-module activeDirectory
Import-Module BitLocker         # Instala el módulo para poder gestionar el BitLocker
```

- `remove-module` para desinstalar módulos
- `get-command -module activeDirectory` obtiene el listado de comando especificos de este paquete.

# 2. PowerShell para administración de *Active Directory*

Uno de los desempeños más habituales de los administradores de sistemas Microsoft Windows Server es la automatización de tareas mediante PowerShell.

Conocer los diferentes comandos PowerShell, es imprescindible para realizar *Scripts* que permitan esta automatización.

## 2.1. Comandos para la gestión de **usuarios**

Con los siguientes usuarios se puede realizar la gestión de todos los elementos que encontramos en la configuración de usuarios del sistema

### 2.1.1. `New-ADUser`

Para crear nuevo usuario

Parámetros habituales:

    - `-UserPrincipalName` : Establace el nombre del Login
    - `-SamAccountName`: Para nombre anterior a Windows 2000
    - `-Name`: Nombre de pila
    - `-Surname`: Apellido/s
    - `-DisplayName`: Nombre a mostrar
    - `-Path` : Ubicación o dirección de árbol AD donde se crea el usuario
    - `-Department` : Nombre del departamento
    - `-AccountPassword (ConvertTo-SecureString -string "P@ssw0rd" -AsPlainText -Force)`: Password
    - `-Enabled`: Si la cuenta esta habilitado o no
    - `-ChangePasswordAtLogon`: Obliga o no a cambiar la contraseña en el primer inicio
    - `-PassThru`: Muestra información detallada en caso de error


***Ejemplos*** de uso: 

```powershell
New-ADUser -UserPrincipalName "adominguez00" -SamAccountName "adominguez00" -Name "Alberto00" -Surname "Dominguez" -DisplayName "Alberto Dominguez" -Path "OU=ouDesarrolloMulti00,OU=ouInformatica00,OU=ouEmpresa00,DC=DOMSERGIO,DC=LAN" -Department "Desarrollo Multiplataforma" -AccountPassword (ConvertTo-SecureString -string "AaAaAa!1" -AsPlainText -Force) -Enabled $true -ChangePasswordAtLogon:$false -PassThru

New-ADUser -DisplayName "alumno1" -Name "alumno1" -UserPrincipalName "alumno1" -Enabled:$true -Path "OU=usuarios,OU=Organizacion,DC=dominio,DC=lan" -AccountPassword (ConvertTo-SecureString -string "P@ssw0rd" -AsPlainText -Force) -ChangePasswordAtLogon:$True
```



### 2.1.2. `Remove-ADuser` 

Para eliminar usuario:

Parámetros: 

    - `-identity` : Para identificar el usuario a modificar

```powershell
Remove-ADuser -identity alumno1 -confirm:$False #para no pedir confirmación
```


### 2.1.3. `Get-ADUser`

Mostrar los usuarios de una unidad organizativa

***Ejemplos*** de uso: 

```powershell
Get-ADUser -identity Alumno1
Get-ADUser -identity Alumno1 -properties *  # Todas las propiedades.
get-aduser -filter * -searchBase "ou=ouAlumnos,dc=domio,dc=lan"
get-ADUser -filter "name -like 'U*'" 
get-ADUser -filter "Name -eq 'Alumno1'"
get-ADUser -filter * | ft Name, UserPrincipalName, Active   # con formato de tabla
get-aduser -filter {name -like "usu*"}  # todos los usuarios que comienzan por usu
get-aduser -filter * | format-table name,userPrincialName,enabled -A # formateamos información en columnas
get-aduser -filter * | format-table name,userPrincialName,enabled -A > listado.txt # Lo mismo de antes, pero lo guardmos en un documento de texto
get-aduser -filter * | ft name,userPrincialName,enabled -A # ojo ft es un alias de format-table
```

**Ejemplo**: Para meter la información de un usuario en una variable
Si metemos usuarios en una variable, después hacer uso en *Scripts*, por ejemplo con `forEach` podemos manipularlos

```powershell
$miusuario=get-ADuser -filter { name -eq "usuario1"] -property *
Write-Host $miusuario.city  # Usamos la variable para acceer
$miusuarios=get-ADuser -filter { name -like "usu*"] -property * # variable que teiene los usuarios
```



### 2.1.4. `Set-ADUser`
Modificar propiedades de los usuarios.

Si cambiamos una propiedad en una variable que tiene asignado un usuario, cambia en la variable que tenemos pero no en el AD.

Para poder actualizar hay que volver a volcar y por esto se hace con un *instance*

```powershell
set-aduser -instance $miusuario
```

Cuando utilizamos el `Set-ADuser` si no fuera por la variable, se debería meter todo el usuario, organización , etc... 

```powershell
Set-ADUser -identity $miusuario -description "Descripcion del usuario"
$misusario.description="Descripcion del usuario" #Otra forma de hacer lo mismo
Set-ADuser -identity $miusuario -department "Informatica" -city "Ayora" -country "ES"
```

Podemos seleccionar un usuario en una variable para trabajar con el
```powershell
$usu3 = Get-ADUser -identity usuario3
$usu3.Name
```



### 2.1.5. `Move-ADUser`
Mover objetos entre Unidades Organizativas

```powershell
Move-ADObject "cn=usuario1,ou=grupo1,dc=dominio,dc=lan" -TargetPath "cn=usuario1,ou=grupo2,dc=dominio,dc=lan"
get-adUser -filter * -searchbase "ou=unidad1,dc=dominio,dc=lan" | Move-AdObject -targetPath "ou=unidad2,dc=domino,dc=lan" # para mover automaticamente¡
get-adUser -filter'name -like "j*" ' -searchbase "ou=unidad1,dc=dominio,dc=lan" | Move-AdObject -targetPath "ou=unidad2,dc=domino,dc=lan" # aplicando filtros
```



### 2.1.6. `Enable-ADAccount` y `Disable-ADAccount`

Habilitar/Deshabilitar un usuario

```powershell
Enable-ADAccount -identity usuario1 -passthru
Disable-ADAccount -identity usuario1
```
Con **-passthru** se fuerza la salida de un resultado por la ejecución del comando.

***Ejemplos*** de uso: Dado un fichero con un listado de usuarios, habilitarlos / deshabilitarlos

```powershell
# Habilitar
cat usuarios.txt | ?{ Enable-ADAccount -identity $_}
# Deshabilitar (sin alias)
Get-Content usuarios.txt | Where-Object { Disable-ADAccount -identity $_}
```

## 2.2. Comandos para la gestión de **grupos** de usuarios


### 2.2.1. `New-ADGroup`: 

Creación de grupo

```powershell 
New-ADGroup -Name "g_alumnos" -SamAccountName "gAlumnos" -GroupCategory Security -GroupScope Global -DisplayName "grupo de alumnos" -Path "OU=grupo,OU=ouAlumnos,DC=dominio,DC=lan" -Description "grupo de alumnos"
```

### 2.2.2. `Remove-ADGroup`: 

Elimina grupo de usuarios

```powershell
Remove-ADGroup -identity gAlumnos
```

### 2.2.3. `Add-ADGRoupMember`

Añadir usuarios a grupos

```powershell
Add-ADGRoupMember -identity grupo -members usuario1
```

### 2.2.4. `Remove-ADGroupMember`

eliminar usuario de grupo

```powershell
Remove-ADGroupMember -identity grupo -members usuario1
```

### 2.2.5. `Get-ADGroupMember`
Consulta de usuarios que pertenecen al grupo administrador

```powershell
$(Get-ADGroupMember -identity "Administradores") | select name
```

### 2.2.6. `Get-AdPrincipalGroupMembemship`

Saber a qué grupos pertenece un usuario

```powershell
Get-AdPrincipalGroupMembemship - identity administrador | Sort-Object | FT -property name, class -A
```
- **FT** : Para formato de tabla Format-Table
- **-A** : parámetro para autoajuste


## 2.3. Comandos para la gestión de **Unidades Organizativas**

### 2.3.1. `New-ADOrganizationalUnit`

Crear una UNIDAD ORGANIZATIVA   

```powershell
New-ADOrganizationalUnit -DisplayName "ouAlumnos" -Name "ouAlumnos" -path "DC=doinio,DC=lan"
```

***Ejemplos*** de uso: Deshabilitando todas las cuenta de una unidad organizativa en concreto
```powershell
get-aduser -SearchBase "OU=ouAlumnos,DC=dominio,DC=lan" -filter * | Disable-ADCount
```

El **_-filter *_** es necesario por que tenemos que tener filtros si o si, entonces es obligatorio ponerlo


### 2.3.2. `get-ADOrganizationalUnit`

para obtener datos de la unidad organizativa

```powershell
get-ADOrganizationalUnit -path "ou=batoi,DC=doinisox,DC=lan"
```

### 2.3.3. `Remove-AdOrganizationaUnit`

Para eliminar unidad organizativa

Por defecto no se pueden eliminar una unidad organizativas, por el parámetro 

- `-ProtectedFromAccidentalDeletion`


> Para ver esto desde la ventana en el entorno visual, dentro del objeto, está el check en propiedades avanzadas

En el caso de powershell también hay que habilitar para que se pueda eliminar

- `-recursive` para borrar todo lo que contenga dentro

***Ejemplos*** de uso: 

```powershell
Set-ADOrganizationalUnit -identity "ou=organizacion,dc=docminio, dc=curso" -ProtectedFromAccidentialDeletion $FAlse
Remove-AdOrganizationaUnit -identity "ou=organization,dc=dominio,dc=curso"
Remove-AdOrganizationaUnit -identity "ou=organization,dc=dominio,dc=curso" -recursive # para borrar todo lo que tenga dentro
```


## 2.4. Comando para la gestión de equipos

### 2.4.1. `New-ADComputer`
Para crear con anterioridad un equipo y que se ubique en un lugar especifico

***Ejemplos*** de uso: 

```powershell
New-ADComputer -name equipo1 -path "ou=organizacion,dc=dominio,dc=curso" -PassThru
```
> **-PassThru** para que muestren detalle de la salida.

### 2.4.2. `Set-ADComputer`
Para modificar un objeto de tipo Computer

```powershell
Set-ADComputer -identity equipo1 -description "PC del usuario 1"
```

### 2.4.3. `Get-ADComputer`
Para seleccionar un computador.

***Ejemplos*** de uso: Para habilitar / deshabilitar: Obtenemos el equipo y lo obtenido se lo pasamos a la order de habilitar o deshabilitar
```powershell
get-ADComputer -filter * -searchbase "ou=uniddad,dc=dominio,dc=curso" | disable-ADAccount
get-ADComputer -filter * -searchbase "ou=uniddad,dc=dominio,dc=curso" | enable-ADAccount
```

### 2.4.4. `Remove-ADComputer`
Borrar equipo

```powershell
remove-ADComputer -identity equipo1
```

## 2.5. Otros comandos de Active Directory

### 2.5.1. `get-ADObject`

Obtiene cualquier tipo de objetos (que filtremos) mientras que el resto de **get-AD..** específica que tipo de objeto queremos


***Ejemplos*** de uso: Listar de unidades organizativas; Buscando en los objetos del dominio y dentro de las unidades organizativas.

```powershell
get-ADObject -filter { objectClass -eq 'organizationalunit'}
get-ADOrganizationalUnit -filter 'name -like "ou*"'  # Lista de unidades organizativas que comienzan por ou  
```


### 2.5.2. `Move-ADObject`
Para poder mover un ordenador de sitio de grupo a otro, o cualquier otro objeto
```powershell
Move-ADObject "cn=equipo1,ou=grupo1,dc=dominio,dc=lan" -TargetPath "cn=equipo1,ou=grupo2,dc=dominio,dc=lan"
```




### 2.5.3. `Get-ADForest`
Conocer el tipo de bosque dentro del bosque genérico

```powershell
Get-ADForest | Select-Object -ExpandProperty ForestMode
Get-ADDoain | Select-Object  -ExpandPropertyh domainMode
```

### 2.5.4. `Get-ADDomainCotroller`
Búsqueda de información del controlador del dominio. EL PC donde esta instalado el dominio. 

```powershell
Get-ADDomainCotroller -filter * 
Get-ADDomainCotroller -filter * | FT name, domain, site, ipv4address, operatingsystem
```

### 2.5.5. `search-ADAccount` 

Búsquedas de cuentas. En este caso tenemos ejemplos de obtener las cuentas inactivas
```powershell
get-aduser -filter {Enabled -eq $false} | select Name, userPrincpalName | Sort Name
search-ADAccount -AccountDisabled -UsersOnly | Format-Table name, objectClass
search-ADAccount -AccountDisabled -UsersOnly | Format-Table name, lasLogonDate, Lockedout, objectclass, passwordexpired, prasswordneverexpires

# Consoltar usuarios/cuentas por fechas
# los que han estado inactivos durante los últimos 9 días.
Search-ADAccount -AccountInactive -TimeSpan 90.00:00:00 -UserOnly | FT Name, ObjectClass  

# que no se han conectado nunca
Get-ADUser -filter * -Properites LastLogonDate | ? { $_.LastLogonName -eq $null } | Select name
```

### 2.5.6. `Set-ADAccountExpiriation` y `Clear-ADAccountExpiration`

Establecer inicio y expiración de las cuentas
```powershell
Set-ADAccountExpiriation -Identity usuario1 -datetime "30/06/2021"
Clear-ADAccountExpiration -identity usuario1 # anualción de fecha de expiración
```

### 2.5.7. `Unlock-ADAccount`

Desbloqueo de cuenta de usuario, por ejemplo al fallar el password.

No hay comando de bloqueo, solo de desbloqueo
```powershell
Unlock-ADAccount -identity usuario1

# busqueda de cuentas bloqueadas y las debloquea automáticamente
search-account -lockedout | unlock-ADAccount -passthru -confirm

# Encuentra los usuarios deshabilitados dentro de un grupo
Get-ADGroupMember -identity "secndariauni10" -Recursive | %{Get-ADUser -identity $_.distingueshedNAme -Propierties Enabled | ....}
```

### 2.5.8. `Set-ADAccountPassword`
Cambio de password de usuario de usuarios

```powershell
Set-ADAccountPassword -identity usuario1 -reset -newpassword (ConvertTo-SecureString -asPlaintText "Pass1234" -Force)
```
- `ConvertTo-SecureString` para cifrar la contraseña que se indica


**Ejemplos** finales: 
- Crear un grupo de seguridad en una ou
```powershell
New-ADGroup -Name "miGrupo" -GroupCategory Security -GroupScope Global -displayName "miGrupo" -Path "OU=ouUsuarios, DC=dominio, DC=lan"
```

- Cambiar un grupo de tipo: Pasarlo a universal de distribución, y además le añade un usuario como adminstrador del grupo
```powershell
Set-ADGroup -identity 'migrupo' -groupcategory Distribution -groupscope Universal -Managedby 'usuario1' 
```

- Obetner un número de usuario de una lista concreta y con una fecha de expiracíon.
```powershell
$(get-ADUser -properties AccountExpirationDate -filter {memberof -recursivematch } | Select name, accountExpirationDate | where ($_.AccountExperitionDate -ne $null).count )
```

## 2.6. Tabla resumen de comandos y sufijos


| Prefijo | Sufijo |
| --- | --- |
| `ADUser` | `New-ADUser` \| `Remove.ADUser` \| `Set-ADUser` \| `Get-ADUser` |
| `ADGroup` | `New-ADGroup` \| `Remove-ADGroup` \| `Set-ADGroup` \| `Get-ADGroup` \| `Add-ADGroupMember` \| `Remove-AdGroupMember` \| `Get-ADGroupMember` |
| `ADOrganizationalUnit` | `New-ADOrganizationalUnit` \| `Remove-ADOrganizationalUnit` \| `Set-ADOrganizationalUnit` \| `Get-ADOrganizationalUnit` | 
| `ADComputer` | `New-ADComputer` \| `Remove-ADComputer` \| `Set-ADComputer` \| `Get-ADComputer` |
| `ADAccount` | `Enable-ADAccount` \| `Disable-ADAccount` \| `Search-ADAccount` \| `Unlock-ADAccount` \| `Set-ADAccountPassword` \| `Set-ADAccountExpiration` \| `Clear-ADAccountExpiration` \| `Set-ADAccountControl` |
| `ADObject` | `New-ADObject` \| `Get-ADObject` \| `Set-ADObject` \| `Sync-ADObject` \| `Move-ADObject` \| `Remove-ADObject` \| `Rename-ADObject` \| `Restore-ADObject` |
| `ADPrincipalGroupMembership` | `Add-AdPrincipalGroupMembership` \| `Set-AdPrincipalGroupMembership` \| `Remove-ADPrincipalGroupMembership` | 
| `ADDomain` | `Set-ADDomain` \| `Get-ADDomain` \| `Set-ADDomainMode` \| `Get-ADDomainController` | 
| `ADForest` | `Set-ADForest` \| `Get-ADForest` \| `Set-ADForestMode` | 




## 2.7. Filtros: Ejemplos de uso con comandos de Active Directory

A continuación batería de consultas a poder realizar con lo visto anteriormente

```powershell
# cuenta todos los usuarios del AD
$(get-aduser -filter *).count

# también funciona
(get-aduser -filter *).count 

# cuenta de una unidad organizativas especifica
$(get-aduser -filter * -searchbase "ou=unidad,dc=dominio=dc=lan").count 

# Adminstradores
$(get-adgroupmember -identity "administradores").count 

# contamos grupos dados de alta en AD
$(get-adgroup -filter * -searchbase "dc=dominio=dc=lan").count  

# Usuarios habilitados
$(get-aduser -filter *  | Where-Object {$_.enabled -eq $True}).count  

# Consulta de usuarios que se han creado durante los últimos 7 días
$fecha = ((get-date).addDays(-7))

# Usuarios habilitados
$(get-aduser -filter * -Properties * | Where-Object {$_.whenCreated -ge $fecha}).count | select Name, whenCreated | Sort Name 
# Eb este caso **-properties** es redundante porque ya forma parte el * (asterisco) del filtro anterior

# Consulta de usuarios que tienen contaseña sin caducidad
$(get-ADUser -filter * -properties * | where{ $_.passwordNeverExpieres -eq $false } ) | select Name | sort Name

# Consultar todos los grupos del sistema
$(get-ADGroup -filter *) | seelect name, groupCategory | FT
```



{% comment %} 

# 3. Active Directory - Permisos

## 3.1. Gestión de permisos SMB

### 3.1.1. `New-SmbShare`
Para asignar premisos SMB, o sea, compartir, una carpeta, utilizamos el comando `New-SmbShare`

Para más información sobre el comando: [Microsoft New-SmbShare](https://learn.microsoft.com/en-us/powershell/module/smbshare/new-smbshare?view=windowsserver2022-ps

Creación de una carpeta y asignar permisos SMB
```powershell
Set-Location e:\                                                                                 
Get-ChildItem                                                                                    
New-Item Desarrollo -ItemType Directory                                                          
New-Item Desarrollo\Web -ItemType Directory                                                      
New-Item Desarrollo\Multiplataforma -ItemType Directory                                          
New-SmbShare -Name Desarrollo -Path E:\Desarrollo\ -FullAccess Administradores -ChangeAccess gDesarrollo00,gDesarrolloMulti00 -ReadAccess gSocios00
```

### 3.1.2. Sufijo `SmbShare`

Como hemos vistos hata ahora, tenemos multiples derivadas del sufijo `SmbShare` como `Get-SmbShare`, `Set-SmbShare` o `Remove-SmbShare`

Consulta el listado completo de comandos en la documentación de [Microsoft SmbShare](https://learn.microsoft.com/es-es/powershell/module/smbshare/?view=windowsserver2022-ps)


## 3.2. Gestión de Permisos NTFS

La gestión de los permisos NTFS depende directamente de la gestión de seguridad del sistema, porque se aplica a los permisos de las carpetas, tanto si se accede localmente o remotamente a traves de SMB. Por ello, la mayoría de la gestión esta incluida en el módulo [Microsoft.PowerShell.Security](https://learn.microsoft.com/es-es/powershell/module/microsoft.powershell.security/?view=powershell-7.3) y en especial en cmdlets como `Get-Acl` y `Set-Acl`

No obstante, para la gestión de l

Los siguentes comando describien como manipular los permisos NTFS de una carpeta

### 3.2.1. `Get-Acl` .Ver los permisos de una carpeta

En primer lugar es esencial tener una forma de ver todos los permisos de una carpeta. 

A continuación vemos la mejor forma. 

Por una parte la función ```Get-ACL``` obtiene el listado de permiso, y mediante el método ```Access``` los muestra. Utilizamos la funcion ```Format-Table``` para visualizar en forma de tabla.
Por último, creamos una variable donde vamos a almacenar la dirección de la carpeta o fichero con el que vamos a trabajar. Esto no es necesario, pero puede servir para reutilizar código de forma sencilla si cambiamos de carpeta o fichero.

```powershell
$Carpeta="c:\CarpetaCompartida"
(Get-ACL -Path $Carpeta).Access | Format-Table IdentityReference, FileSystemRights, AccessControlType, IsInherited, InheritanceFlags -AutoSize
```

### 3.2.2. Deshabilitar la herencia

Para poder eliminar ciertos permisos debemos deshacer la herencia de los mismos.
Lo realizamos mediante la funcion ```SetAccessRuleProtection``` tal y como vemos a continuación.
Como podemos ver, en primer lugar metemos en una variable el listado de permisos que obtenemos mediante la funcion ```Get-ACL``` y después una vez eliminados los permisos en la variable, se vuelven a volcar o aplicar al recurso inicial mediante la función ```Set-ACL```

```powershell
$ACL = Get-Acl -Path $Carpeta                                                             
$ACL.SetAccessRuleProtection( $true, $true)                                                      
$ACL | Set-Acl -Path $Carpeta                                                             
```

### 3.2.3. Eliminar todos los permisos de un usuario

Para eliminar los permisos de un recurso, comenzamos como siempre obteniendo en una variable todos los permisos, y mediante el filtro ```Where-Object``` filtramos y obtenemos los permisos concretos que queremos eliminar.

La eliminación se realiza mediante la función ```Acl-RemoveAccessRule```, y posteriormente, como en el caso anteior, volvemos a asignar los nuevos permisos al recurso inicial

```powershell
$Acl = Get-Acl -Path $Carpeta
$AccessRule = $Ace.Access | Where-Object {($_.IdentityReference -eq 'BUILTIN\Usuarios') -and -not ($_.IsInherited)}
$Acl.RemoveAccessRule( $AccessRule)
$Acl | Set-Acl -Path $Carpeta
$Acl.Access | Format-Table IdentityReference, FileSystemRights, AccessControlType, IsInherited, InheritanceFlags -AutoSize
```

### 3.2.4. Añadir permisos a un usuario

Para añadir un nuevo permiso a una carpeta, creamos el permiso mediante la función ```New-Object System.Security.AccessControl.FileSystemAccessRule()``` y asígnamos en orden los valores para el usuario o grupo de seguridad, los permisos que tiene, opciones de herencia y de propagación, y el tipo de permiso que se esta aplicando, que puede ser de permiso o de denegación del mismo.
Una vez creado el nuevo permiso, se añade al listado ACL de la carpeta y por último, como siempre, se reescriben todos los permisos de la carpeta

```powershell
$Acl = Get-Acl -Path $Carpeta
$AccessRule = New-Object System.Security.AccessControl.FileSystemAccessRule(.....) # Explicación más abajo
$Acl.AddAccessRule( $AccessRule)
$ACL | Set-Acl -Path $Carpeta
```

### 3.2.5. Funcion **```New-Object System.Security.AccessControl.FileSystemAccessRule()```**

Función que crea un permiso específico NTFS.

A continuación se detallan los posibles valores que pueden tomar cada uno de los parámetros de la funcioń

| Parámetro | Valores |
| --- | --- |
| ***name*** | Usuario o grupo de seguridad |
| ***FileSystemRights*** | ListDirectory, ReadData, WriteData, CreateFiles, CreateDirectories, AppendData, ReadExtendedAttributes, WriteExtendedAttributes, Traverse, ExecuteFile, DeleteSubdirectoriesAndFiles, ReadAttributes, WriteAttributes, Write, Delete, ReadPermissions, Read, ReadAndExecute, Modify, ChangePermissions, TakeOwnership, Synchronize, FullControl | 
| ***InheritanceFlags*** | None, ContainerInherit, ObjectInherit | 
| ***PropagationFlags*** | None, NoPropagateInherit, InheritOnly | 
| ***AccessControlType*** | Allow, Deny | 


{% endcomment %}
