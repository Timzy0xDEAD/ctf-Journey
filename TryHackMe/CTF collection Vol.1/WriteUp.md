
# TryHackMe – CTF collection Vol.1 Write-up

### Task 2: 
 echo "VEhNe2p1NTdfZDNjMGQzXzdoM19iNDUzfQ==" | base64 -d 
### Task 3:
 exiftool Find_me_1577975566801.jpg 
### Task 4:
 steghide info Extinction_1577976250757.jpg

 steghide extract -sf Extinction_1577976250757.jpg

 cat Final_message.txt
### Task 5:
 code source inspection
### Task 6:
 zbarimg QR_1577976698747.png 
### Task 7:
 file hello_1577977122465.hello

 strings hello_1577977122465.hello
### Task 8:
 use CyberChef : https://cyberchef.org/
### Task 9:
 A Caesar cipher shifted by 7

 echo "MAF{atbe_max_vtxltk}" | tr 'a-zA-Z' 'h-za-gH-ZA-G'
### Task 10:
 check the comments in html 
### Task 11:
 exiftool spoil_1577979329740.png
 
 file spoil_1577979329740.png

 xxd spoil_1577979329740.png | head

 printf '\x89\x50\x4E\x47' | dd of=spoil_1577979329740.png bs=1 seek=0 count=4 conv=notrunc

 open spoil_1577979329740.png 
### Task 12:
 Check Tryhackme social account in reddit
### Task 13:
### Task 14:
 echo "44585d6b2368737c65252166234f20626d" | xxd -r -p | xxd -b
 
 Use https://www.dcode.fr/chiffre-xor  for S1 XOR S2
### Task 15:
 binwalk hell_1578018688127.jpg  

 binwalk -e hell_1578018688127.jpg  

 cd _hell_1578018688127.jpg.extracted 

 cat hello_there.txt  
### Task 16:
 java -jar Stegsolve.jar
 java -jar Stegsolve.jar dark_1578020060816.png 
### Task 17:
### Task 18:
### Task 19:
 use https://www.dcode.fr/vigenere-cipher
### Task 20:
 echo "obase=16; 581695969015253365094191591547859387620042736036246486373595515576333693" | bc | xxd -r -p 
### Task 21:







 








