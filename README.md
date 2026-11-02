
~/webdemo/easybuybd $ python - <<'PY'
e),"_blank");
}
showProducts();
</script>'''
)
p.write_text(s)
print("WhatsApp order button added!")
PY> from pathlib import Path
> p = Path("index.html")
> s = p.read_text()
> s = s.replace(
> '</style>',
> '.order-btn{background:#128c4a;color:white;border:0;padding:14px;width:100%;border-radius:8px;font-size:17px;font-weight:bold;margin-top:12px}\n</style>'
> )
> s = s.replace(
> '<h3 id="total">মোট: ৳০</h3>',
> '<h3 id="total">মোট: ৳০</h3>\n<button class="order-btn" onclick="orderWhatsApp()">WhatsApp-এ অর্ডার করুন</button>'
> )
> s = s.replace(
> 'function showProducts(){',
> 'function showProducts(){'
> )
> s = s.replace(
> 'showProducts();\n</script>',
> '''function orderWhatsApp(){
> if(cart.length===0){alert("আগে কার্টে পণ্য যোগ করুন");return;}
> let message="আমি Easy Buy BD থেকে অর্ডার করতে চাই:\\n";
> cart.forEach(p=>message+=p.name+" - ৳"+p.price+"\\n");
> message+="মোট: ৳"+cart.reduce((sum,p)=>sum+p.price,0);
> window.open("https://wa.me/8801705512676?text="+encodeURIComponent(message),"_blank");
> }
> showProducts();
> </script>'''
> )
> p.write_text(s)
> print("WhatsApp order button added!")
> PY
WhatsApp order button added!
~/webdemo/easybuybd $ cat > admin.html <<'EOF'
">
<input id="image" placeholder="ছবির URL (ঐচ্ছিক)">
<button onclick="addProduct()">পণ্য যোগ করুন</button>
<h3>আপনার পণ্যসমূহ</h3>
<div id="list"></div>
</main>
<script>
const key="easybuy_products";
let products=JSON.parse(localStorage.getItem(key)||"[]");
function save(){localStorage.setItem(key,JSON.stringify(products));show();}
function addProduct(){
let name=document.getElementById("name").value.trim();
let price=Number(document.getElementById("price").value);
let image=document.getElementById("image").value.trim();
if(!name||price<=0){alert("পণ্যের নাম ও সঠিক দাম লিখুন");return;}
products.push({name,price,image,category:"অন্যান্য"});
save();
document.getElementById("name").value="";
document.getElementById("price").value="";
document.getElementById("image").value="";
alert("পণ্য যোগ হয়েছে!");
}
function show(){
document.getElementById("list").innerHT> <!DOCTYPE html>
> <html lang="bn">
> <head>
> <meta charset="UTF-8">
> <meta name="viewport" content="width=device-width,initial-scale=1">
> <title>Easy Buy BD Admin</title>
> <style>
> body{font-family:Arial;background:#f1f5f3;margin:0;padding:18px}
> main{max-width:550px;margin:auto;background:white;padding:20px;border-radius:14px}
> h2{color:#087b50;text-align:center}
> input,button{width:100%;padding:13px;margin:8px 0;box-sizing:border-box;border-radius:7px;font-size:16px}
> input{border:1px solid #ccc}
> button{background:#087b50;color:white;border:0}
> .product{padding:12px;border:1px solid #ddd;margin:10px 0;border-radius:8px}
> .delete{background:#c62828}
> </style>
> </head>
> <body>
> <main>
> <h2>Easy Buy BD — Admin Panel</h2>
> <h3>নতুন পণ্য যোগ করুন</h3>
> <input id="name" placeholder="পণ্যের নাম">
> <input id="price" type="number" placeholder="পণ্যের দাম (টাকা)">
> <input id="image" placeholder="ছবির URL (ঐচ্ছিক)">
> <button onclick="addProduct()">পণ্য যোগ করুন</button>
> <h3>আপনার পণ্যসমূহ</h3>
> <div id="list"></div>
> </main>
> <script>
> const key="easybuy_products";
> let products=JSON.parse(localStorage.getItem(key)||"[]");
> function save(){localStorage.setItem(key,JSON.stringify(products));show();}
> function addProduct(){
> let name=document.getElementById("name").value.trim();
> let price=Number(document.getElementById("price").value);
> let image=document.getElementById("image").value.trim();
> if(!name||price<=0){alert("পণ্যের নাম ও সঠিক দাম লিখুন");return;}
> products.push({name,price,image,category:"অন্যান্য"});
> save();
> document.getElementById("name").value="";
> document.getElementById("price").value="";
> document.getElementById("image").value="";
> alert("পণ্য যোগ হয়েছে!");
> }
> function show(){
> document.getElementById("list").innerHTML=products.map((p,i)=>
> `<div class="product"><b>${p.name}</b><br>দাম: ৳${p.price}<br><button class="delete" onclick="removeProduct(${i})">মুছে ফেলুন</button></div>`
> ).join("")||"এখনো কোনো পণ্য যোগ করা হয়নি।";
> }
> function removeProduct(i){if(confirm("পণ্যটি মুছে ফেলবেন?")){products.splice(i,1);save();}}
> show();
> </script>
> </body>
> </html>
> EOF
~/webdemo/easybuybd $ python -m http.server 8001 --bind 127.0.0.1
Serving HTTP on 127.0.0.1 port 8001 (http://127.0.0.1:8001/) ...
127.0.0.1 - - [03/Nov/2026 00:24:50] "GET /admin.html HTTP/1.1" 200 -
127.0.0.1 - - [03/Nov/2026 00:24:50] code 404, message File not found
127.0.0.1 - - [03/Nov/2026 00:24:50] "GET /favicon.ico HTTP/1.1" 404 -
127.0.0.1 - - [03/Nov/2026 00:24:52] "GET /admin.html HTTP/1.1" 304 -

^C
Keyboard interrupt received, exiting.
~/webdemo/easybuybd $ cd ~/webdemo/easybuybd
~/webdemo/easybuybd $ ls -l
total 12
-rw-------. 1 u0_a389 u0_a389 2382 Nov  3 00:24 admin.html
-rw-------. 1 u0_a389 u0_a389 5953 Nov  3 00:19 index.html
~/webdemo/easybuybd $ pkg install git -y
Hit:1 https://termux.net stable InRelease
All packages are up to date.
Installing:
  git

Installing dependencies:
  krb5  libdb    libresolv-wrapper  openssh-sftp-server
  ldns  libedit  openssh            termux-auth

Suggested packages:
  perl  termux-services

Summary:
  Upgrading: 0, Installing: 9, Removing: 0, Not Upgrading: 0
  Download size: 7779 kB
  Space needed: 46.1 MB

Get:1 https://termux.net stable/main aarch64 git aarch64 2.54.0 [5022 kB]
Get:2 https://termux.net stable/main aarch64 libresolv-wrapper aarch64 1.1.7-6 [11.3 kB]
Get:3 https://termux.net stable/main aarch64 libdb aarch64 18.1.40-5 [499 kB]
Get:4 https://termux.net stable/main aarch64 krb5 aarch64 1.22.2 [897 kB]
Get:5 https://termux.net stable/main aarch64 ldns aarch64 1.8.4-1 [303 kB]
Get:6 https://termux.net stable/main aarch64 libedit aarch64 20240517-3.1-1 [79.2 kB]
Get:7 https://termux.net stable/main aarch64 openssh-sftp-server aarch64 10.3p1-1 [55.8 kB]
Get:8 https://termux.net stable/main aarch64 termux-auth aarch64 1.5.0-1 [7020 B]
Get:9 https://termux.net stable/main aarch64 openssh aarch64 10.3p1-1 [905 kB]
Fetched 7779 kB in 4s (2060 kB/s)
Selecting previously unselected package git.
(Reading database ... 14474 files and directories currently installed.)
Preparing to unpack .../0-git_2.54.0_aarch64.deb ...
Unpacking git (2.54.0) ...
Selecting previously unselected package libresolv-wrapper.
Preparing to unpack .../1-libresolv-wrapper_1.1.7-6_aarch64.deb ...
Unpacking libresolv-wrapper (1.1.7-6) ...
Selecting previously unselected package libdb.
Preparing to unpack .../2-libdb_18.1.40-5_aarch64.deb ...
Unpacking libdb (18.1.40-5) ...
Selecting previously unselected package krb5.
Preparing to unpack .../3-krb5_1.22.2_aarch64.deb ...
Unpacking krb5 (1.22.2) ...
Selecting previously unselected package ldns.
Preparing to unpack .../4-ldns_1.8.4-1_aarch64.deb ...
Unpacking ldns (1.8.4-1) ...
Selecting previously unselected package libedit.
Preparing to unpack .../5-libedit_20240517-3.1-1_aarch64.deb ...
Unpacking libedit (20240517-3.1-1) ...
Selecting previously unselected package openssh-sftp-server.
Preparing to unpack .../6-openssh-sftp-server_10.3p1-1_aarch64.deb ...
Unpacking openssh-sftp-server (10.3p1-1) ...
Selecting previously unselected package termux-auth.
Preparing to unpack .../7-termux-auth_1.5.0-1_aarch64.deb ...
Unpacking termux-auth (1.5.0-1) ...
Selecting previously unselected package openssh.
Preparing to unpack .../8-openssh_10.3p1-1_aarch64.deb ...
Unpacking openssh (10.3p1-1) ...
Setting up libedit (20240517-3.1-1) ...
Setting up openssh-sftp-server (10.3p1-1) ...
Setting up ldns (1.8.4-1) ...
Setting up git (2.54.0) ...
Setting up libresolv-wrapper (1.1.7-6) ...
Setting up termux-auth (1.5.0-1) ...
Setting up libdb (18.1.40-5) ...
Setting up krb5 (1.22.2) ...
Setting up openssh (10.3p1-1) ...
Generating public/private rsa key pair.
Your identification has been saved in /data/data/com.termux/files/usr/etc/ssh/ssh_host_rsa_key
Your public key has been saved in /data/data/com.termux/files/usr/etc/ssh/ssh_host_rsa_key.pub
The key fingerprint is:
SHA256:I2Nu3iaj3wkp0VLpBJVk9cg/J5qnF7Y+xPsVFEaKv/k u0_a389@localhost
The key's randomart image is:
+---[RSA 3072]----+
|    .o+o.    .+  |
|     o.o o . o . |
|      + o o . .  |
|     =   . . .   |
|    o * S.+ o .  |
|     = + +=+ o . |
|    . = oo.+o .  |
|     +o+.+=  o   |
|    .oo+=o.o. E  |
+----[SHA256]-----+
Generating public/private ecdsa key pair.
Your identification has been saved in /data/data/com.termux/files/usr/etc/ssh/ssh_host_ecdsa_key
Your public key has been saved in /data/data/com.termux/files/usr/etc/ssh/ssh_host_ecdsa_key.pub
The key fingerprint is:
SHA256:2puDziE7vgrWscRSVRQeSWBa8ZmCdSvtzJy+iKgV9DU u0_a389@localhost
The key's randomart image is:
+---[ECDSA 256]---+
|     B*Bo        |
|    B =.=        |
|  .+ oEB         |
| .o. .B..        |
| ..+.  *S        |
|  +.o .o         |
|...o. oo.        |
|.o. o+o.oo       |
|o..++=o.o.       |
+----[SHA256]-----+
Generating public/private ed25519 key pair.
Your identification has been saved in /data/data/com.termux/files/usr/etc/ssh/ssh_host_ed25519_key
Your public key has been saved in /data/data/com.termux/files/usr/etc/ssh/ssh_host_ed25519_key.pub
The key fingerprint is:
SHA256:nYjXJjBujplBNd8Oo29EWlff0gFKV42GE76CpfMdZjc u0_a389@localhost
The key's randomart image is:
+--[ED25519 256]--+
|      o    .o=o+.|
|     . o ..o=.ooo|
|    . o * +..oo o|
|   . . O % . . . |
|    . * S B = E  |
|     B + = = o . |
|    + . o . .    |
|       .         |
|                 |
+----[SHA256]-----+

If you plan to use the 'ssh-agent'
it is recommended to run it as a service.
Run 'pkg i termux-services'
to install the ('runit') service manager

You can enable the ssh-agent service
using 'sv-enable ssh-agent'
You can also enable sshd to autostart
using 'sv-enable sshd'
~/webdemo/easybuybd $
