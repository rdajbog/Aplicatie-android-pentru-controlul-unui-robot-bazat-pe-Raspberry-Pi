 Aplicatie android pentru controlul unui robot bazat pe Raspberry Pi4

Acest proiect reprezintă o lucrare de licență ce vizează dezvoltarea unui sistem robotizat mobil, multifuncțional, capabil să efectueze monitorizarea calității aerului și supraveghere video inteligentă. Sistemul integrează o arhitectură hibridă (Raspberry Pi 4B + Arduino Uno) și este controlat printr-o aplicație Android nativă, oferind interacțiune multimodală: tactilă, vocală și prin gesturi. 
Obiectivul principal a fost depășirea limitărilor platformelor educaționale prin crearea unui robot, capabil de procesare de imagine în timp real și analiză a datelor senzoriale.

Arhitectura:
Unitate centrală de procesare – Raspberry Pi 4B (procesare imagine + server Flask) :
            Funcționează ca "creierul" robotului.
            Rulează un server Flask pentru API REST și streaming video via HTTP/WebSocket.
            Gestionează procesarea imaginilor folosind OpenCV și MediaPipe (Recunoaștere Gesturi).
            Comunică cu aplicația Android prin Wi-Fi.
Control motoare și senzori – Arduino Uno + driver L298N
            Controlează direct driverele de motor (L298N) și sistemul de propulsie 4WD.
            Citește datele analogice de la senzorul de gaz MQ-9.
            Gestionează afișajul local OLED pentru feedback vizual (ex. ochi animați).
            Comunică cu Raspberry Pi prin protocol Serial (USB).
            
<img width="810" height="693" alt="image" src="https://github.com/user-attachments/assets/0d52c10d-fdc1-4e76-902d-b286e298a222" />
<img width="1398" height="280" alt="image" src="https://github.com/user-attachments/assets/dd09aca6-057a-4178-b210-a68c495746f6" />

Monitorizare Ambientală:
Senzor gaz – MQ-9 (conectat la Arduino, valori trimise către Raspberry Pi)
            Utilizează senzorul MQ-9 pentru detectarea monoxidului de carbon și a gazelor inflamabile.
            Conversie în timp real a valorilor ADC în PPM (Parts Per Million) folosind algoritmi de calibrare logaritmică.
            Alertare vizuală în aplicație
<img width="920" height="252" alt="image" src="https://github.com/user-attachments/assets/e2ea47ea-8481-46bc-b304-dd50f512a05a" />
<img width="738" height="96" alt="image" src="https://github.com/user-attachments/assets/808d3ef6-d0a1-45b3-88c4-638f9dec1917" />
<img width="1225" height="497" alt="image" src="https://github.com/user-attachments/assets/da70432f-2765-4a8e-8d9c-d4ce29b23e50" />

Interfață locală – Ecran OLED 1.3" cu expresii animate
            Ecran OLED montat pe robot care simulează expresii faciale (clipește, privește stânga/dreapta) în funcție de acțiuni.
<img width="494" height="659" alt="image" src="https://github.com/user-attachments/assets/0fa1905d-1133-4834-abff-80bf6f874f96" />

Comunicare – Server Flask pe Raspberry Pi + aplicație Android (HTTP/WebSocket)
Aplicație Android – dezvoltată în Android Studio (Java/Kotlin)

Decizii de Proiectare
De ce Raspberry Pi 4 + Arduino în loc de ESP32-CAM?
Inițial, proiectul a fost conceput pe baza unui ESP32-CAM. Totuși, în timpul dezvoltării, au fost identificate limitări critice care au dus la o refactorizare majoră a arhitecturii.
<img width="990" height="994" alt="image" src="https://github.com/user-attachments/assets/96b3c10b-e3d3-4f05-a553-f6b5f23b1cbc" />
<img width="1199" height="693" alt="image" src="https://github.com/user-attachments/assets/38b61cbd-18be-40be-8717-a9b391363af5" />

<img width="568" height="597" alt="image" src="https://github.com/user-attachments/assets/797a76b9-da93-4508-be51-d1dae2d9ce36" /> Calitatea camerei ESP32-CAM
VS 
<img width="680" height="519" alt="image" src="https://github.com/user-attachments/assets/b9930b84-cdf5-43bc-9554-efab1e5b48f6" /> Calitatea camerei Camera Module v3


Aplicația Android
Interfața utilizator a fost dezvoltată folosind Material Design din Androidstudio.
            streaming: webView optimizat pentru latență minimă (<300ms).
            comunicare: cereri HTTP către serverul Flask.
            securitate: autentificare pentru accesul la comenzile robotului.


Instalare și Configurare:
Pentru a pune sistemul în funcțiune, urmează acești pași în ordine
1.Pornirea Hardware 
            Asigură-te că Raspberry Pi 4 este alimentat (la bateria externă).
            Asigură-te că Arduino Uno este alimentat (la bateriile Li-Po).
            Așteaptă 30-60 de secunde pentru ca sistemul de operare (Raspberry Pi OS) să pornească complet.
2.Pornirea Serverului
Accesează terminalul Raspberry Pi (direct sau prin SSH) și rulează scriptul principal al serverului. Acesta va inițializa camera, comunicarea cu Arduino și va deschide portul pentru conexiune
După rulare, scriptul va genera și afișa în terminal Adresa IP locală a robotului (ex: 192.168.1.X)

3.Conectarea clientului:
Acum ca serverul rulează și așteaptă conexiuni:

Deschide proiectul în Android Studio (sau instalează aplicația pe telefon).
            Lansează aplicația.
            Mergi la meniul de Setări Conexiune.
            Introdu Adresa IP generată anterior de Raspberry Pi și Portul 5000.
            Apasă Conectare.
Dacă totul este configurat corect, vei primi mesajul "Conectat cu succes" și vei vedea fluxul video de la cameră.
