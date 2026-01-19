 Aplicatie android pentru controlul unui robot bazat pe Raspberry Pi4

Acest proiect reprezinta o lucrare de licenta ce vizeaza dezvoltarea unui sistem robotizat mobil, multifunctional, capabil sa efectueze monitorizarea calitatii aerului si supraveghere video inteligenta. Sistemul integreaza o arhitectura hibrida (Raspberry Pi 4B + Arduino Uno) si este controlat printr-o aplicatie Android, oferind interactiune multimodala: tactila, vocala si prin gesturi. 
Obiectivul principal a fost depasirea limitarilor platformelor educationale prin crearea unui robot, capabil de procesare de imagine in timp real si analiza a datelor senzoriale.

Arhitectura:
Unitate centrala de procesare – Raspberry Pi 4B (procesare imagine + server Flask) :
            Functioneaza ca "creierul" robotului.
            Ruleaza un server Flask pentru API REST si streaming video prin HTTP/WebSocket.
            Gestioneaza procesarea imaginilor folosind OpenCV si MediaPipe (Recunoastere gesturi).
            comunica cu aplicatia android prin wifi.

Control motoare si senzori – Arduino Uno + driver L298N
            Controleaza direct driverele de motor (L298N) si sistemul de propulsie 4WD.
            Citeste datele analogice de la senzorul de gaz MQ-9.
            Gestioneaza afisajul local OLED pentru feedback vizual (ex. ochi animati).
            Comunica cu Raspberry Pi prin protocol seriala.
            
<img width="810" height="693" alt="image" src="https://github.com/user-attachments/assets/0d52c10d-fdc1-4e76-902d-b286e298a222" />
<img width="1398" height="280" alt="image" src="https://github.com/user-attachments/assets/dd09aca6-057a-4178-b210-a68c495746f6" />

Monitorizare Ambientala:
Senzor gaz – MQ-9 (conectat la Arduino, valori trimise catre Raspberry Pi)
            Utilizeaza senzorul MQ-9 pentru detectarea monoxidului de carbon si a gazelor inflamabile.
            Conversie in timp real a valorilor ADC in PPM (Parts Per Million).
            Alertare vizuala in aplicatie
<img width="920" height="252" alt="image" src="https://github.com/user-attachments/assets/e2ea47ea-8481-46bc-b304-dd50f512a05a" />
<img width="738" height="96" alt="image" src="https://github.com/user-attachments/assets/808d3ef6-d0a1-45b3-88c4-638f9dec1917" />
<img width="1225" height="497" alt="image" src="https://github.com/user-attachments/assets/da70432f-2765-4a8e-8d9c-d4ce29b23e50" />

Interfata locala – Ecran OLED 1.3" cu expresii animate
            Ecran OLED montat pe robot care simuleaza expresii faciale (clipeste, priveste stanga/dreapta) in functie de actiuni.
<img width="494" height="659" alt="image" src="https://github.com/user-attachments/assets/0fa1905d-1133-4834-abff-80bf6f874f96" />

Comunicare – Server Flask pe Raspberry Pi + aplicatie Android (HTTP/WebSocket)
Aplicatie android – dezvoltata in Android Studio (Java/Kotlin)

Decizii de proiectare
De ce Raspberry Pi 4 + Arduino in loc de ESP32-CAM?
Initial, proiectul a fost conceput pe baza unui ESP32-CAM. Totusi, in timpul dezvoltarii, au fost identificate limitari critice care au dus la o refactorizare majora a arhitecturii.
<img width="990" height="994" alt="image" src="https://github.com/user-attachments/assets/96b3c10b-e3d3-4f05-a553-f6b5f23b1cbc" />
<img width="1199" height="693" alt="image" src="https://github.com/user-attachments/assets/38b61cbd-18be-40be-8717-a9b391363af5" />
Calitatea camerei ESP32-CAM:
<img width="568" height="597" alt="image" src="https://github.com/user-attachments/assets/797a76b9-da93-4508-be51-d1dae2d9ce36" /> 

 Calitatea camerei Camera Module v3:
<img width="680" height="519" alt="image" src="https://github.com/user-attachments/assets/b9930b84-cdf5-43bc-9554-efab1e5b48f6" /> 


Aplicatia Android
Interfata utilizator a fost dezvoltata folosind Material Design din Androidstudio.
            streaming: webView optimizat pentru latenta minima (<300ms).
            comunicare: cereri HTTP către serverul Flask.
            securitate: autentificare pentru accesul la comenzile robotului.


Instalare si configurare:
Pentru a pune sistemul in functiune, se vor urmarii acesti pasi in ordine
1.Pornirea Hardware 
            Asigura-te ca Raspberry Pi 4 este alimentat (la bateria externa).
            Asigura-te ca Arduino Uno este alimentat (la bateriile Li-Po).
            Asteapta 30-60 de secunde pentru ca sistemul de operare (Raspberry Pi OS) sa porneasca complet.
            
2.Pornirea Serverului
Acceseaza terminalul Raspberry Pi (direct sau prin SSH) si ruleaza scriptul principal al serverului. Acesta va initializa camera, comunicarea cu Arduino si va deschide portul pentru conexiune
Dupa rulare, scriptul va genera si afisa in terminal Adresa IP locala a robotului (ex: 192.168.1.X)

3.Conectarea clientului:
Acum ca serverul ruleaza si asteapta conexiuni:

Deschide proiectul in Android Studio (sau instaleaza aplicatia pe telefon).
            Lanseaza aplicatia.
            Mergi la meniul de Setari Conexiune.
            Introdu Adresa IP generata anterior de Raspberry Pi si Portul 5000.
            Apasa Conectare.
Daca totul este configurat corect, vei primi mesajul "Conectat cu succes" si vei vedea fluxul video de la camera.
