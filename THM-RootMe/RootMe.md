<h1>RootMe Walktrought</h1> 

<img src="images/icon.png">

TryHackMe platformunda yer alan bu makinenin çözüm yolu adım adım gösterilmektedir.
Makineye <a href="https://tryhackme.com/r/room/rrootme">buradan</a> ulaşabilrisiniz
>
<p>
<h4>1 Rustscan:</h4>
Makineyi platform üzerinden başlattıktan sonra verilen makine 
IP sine açık portların keşfedilmesi için için RustScan aracı kullanılmıştır.
22 ve 80 portlarının açık olduğu görüldü.
</p>
<br>
<div style="text-align: center;">
<img src="images/1-rustscan.png" alt="1-rustscan" width="400" " >
</div>
<br>
<p>2 Nmap:
    Açık olan portlar belirlendikten sonra Nmap ile portlarda çalışan servisleri, servislerin sürümlerini ve default scriptler çalıştırılarak makine hakkında daha detaylı bilgi toplanıldı.
    <br>
</p>
```bash nmap  -p 22,80 -sC -Pn -sV -T5  10.10.156.243
<br>
<div style="text-align: center;">
<img src="images/2-nmap.png" alt="1-rustscan" width="500" >
</div>

<br>
<br>

| flag          | Anlamı            |
| ------------- |:-------------:|
|-p     | Sadece belirtilen portlarda tarama yapmak için kullanılır.|
| -sC   | Portlarda çalışan servisleri için tanımlanmış Scriptleri çalıştırarak bilgi toplar.|
| -sV   | Hedefteki servislerin sürümlerini tespit edilmesini sağlar |
| -Pn   | Hedefe ICMP ping gönderilmemesi için kullanılır.|
| -T5   | Tarama hızını belirler ‘5’ en hızlı seviyedir.|



<p>
3 Dirsearch
Web servisindeki gizli dizin ve dosyaları bulunması için dizin taraması yapıldı. Dizin taraması için ‘dirsearch’ aracı kullanıldı ve aracın default wordlist kullanıldı. 

<br>
    <div style="text-align: center;">
    <img src="images/3-dirsearch.png" alt="3" width="450" height="350" >
    </div>
<br>

</p>
<p>
4  Yapılan dizin taramasında bulunan “/panel” dizini kontrol edildiğinde bir dosya yükleme sayfası olduğu görüldü. 


<br>
    <div style="text-align: center;">
    <img src="images/7-uploadshell.png" alt="3" width="600" height="250" >
    </div>
<br>

</p>
<p>
    ilk olarak bir ReverseShell içeren bir php dosyası yüklemeye çalışıldı fakat  yükleme kısmında php uzantısına izin verilmediği görüldü
    <br>
    <div style="text-align: center;">
    <img src="images/8-errormessage.png" alt="3" width="300" >
    </div>
<br>
</p>
<p>
    Php dosyası gibi çalışan fakat uzantısı php olmayan uzantıları bulmak için yapılan aramada karşımıza çıkan sitede bazı uzantılar bulundu.
<br>
    <div style="text-align: center;">
    <img src="images/9-phpextensions.png" alt="3" width="500" >
    </div>
<br>

</p>

<p>
    Hangi uzantının işe yaradığını bulmak için Burpsuite açarak, dosya yükleme isteğini yakalayıp istekte bulunan dosya uzantı kısmındaki php  seçilerek, istek  intruder a gönderildi.
<br>
    
<img src="images/10-extensionbypass.png" alt="3" width="600"  >
  
<br>
</p>
<p>
    İntruder sekmesindeli sniper kısmına, sitede bulunan uzantılar liste halinde eklenerek farklı uzantılar ile istek tekrar tekrar gönderildi. 
    <div style="text-align:center;">
    <img src="images/11-extensionlist.png" alt="3" width="550" >
    <br>
    <br>
    <p>
        Gönderilen isteklerin sonuçları kontrol edildiğinde .phtml uzantısının başarılı bir şekilde siteye yüklendiği görüldü.
    <br>
    <img src="images/12-resultcompare.png" alt="3" width="750" >
    </p>
</div>
<br>
</p>

<p>
    Terminal ekranından netcat aracı ile yüklenen reverseshell deki port dinlemeye alınarak, browserdan sitedeki yüklenen dosyaya istek gönderildiğinde Shell bağlantısının gerçekleştiği görüldü.
<br>
    <div style="text-align: center;">
    <img src="images/13-shellconnect.png" alt="3" width="750" height="100" >
    </div>
<br>
</p>
<p>
    İlk bayrağı bulmak için çağırılan komut ile bayrağın dizini öğrenilip bayrak yakalanmıştır.
<br>
    <div style="text-align: center;">
    <img src="images/14-usertxt.png" alt="3" width="400"  >
    </div>
<br>
</p>

<p>
    Yetki yükseltmek için çalıştırılan kod ile sahibi root olan setuid biti ayarlanmış olan dosyaları listelendi bu dosyalar araştırılarak hangisi ile root yetkisinde bir shell alınabileceği belirlendi. 
<br>
    <div style="text-align: center;">
    <img src="images/15-SUID.png" alt="3" width="400" >
    </div>
<br>
</p>


<p>
    GFTOBins sitesinden belirlenen tool ile yetki yükseltmeye yaracak kodlar makinede çalıştırılarak yetki yükseltildi ve root yetkisinde bir bağlantı elde edildi. İkinci bayrak /root dizini altında bulunarak, bayrak yakalandı.
    <div style="text-align: center;">
    <img src="images/16-gtfobins.png" alt="3" width="500" height="200" >
    <img src="images/17-PESroot.png" alt="3" width="500" >
    </div>
<br>
</p>

