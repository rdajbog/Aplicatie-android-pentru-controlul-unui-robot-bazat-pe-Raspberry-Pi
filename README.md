 Aplicatie android pentru controlul unui robot bazat pe Raspberry Pi4

Acest proiect reprezinta o lucrare de licenta ce vizeaza dezvoltarea unui sistem robotizat mobil, multifunctional, capabil sa efectueze monitorizarea calitatii aerului si supraveghere video inteligenta. Sistemul integreaza o arhitectura hibrida (Raspberry Pi 4B + Arduino Uno) si este controlat printr-o aplicatie Android, oferind interactiune multimodala: tactila, vocala si prin gesturi. 
Obiectivul principal a fost depasirea limitarilor platformelor educationale prin crearea unui robot, capabil de procesare de imagine in timp real si analiza a datelor senzoriale.

Interfata robot : 
<img width="447" height="881" alt="image" src="https://github.com/user-attachments/assets/9e93a5ab-4d47-4229-bf7d-5a69d5ee19c5" />

Meniu principal :
<img width="448" height="862" alt="image" src="https://github.com/user-attachments/assets/6f6a6d63-c664-486d-84c0-7e7f2e98bea2" />

Interfata control robot:


1.Controlul mișcării robotului:
Această sectiune include butoane dedicate controlului direct al propulsiei robotului prin Arduino Uno (via Raspberry Pi). Butoanele sunt aranjate in butoane direcționale. Fiecare buton trimite o cerere HTTP către serverul Flask, care o redirecționează către Arduino prin comunicație serială UART.
ex: Buton "Înainte" : Activeaza mișcarea înainte a robotului prin cele 4 motoare DC. Endpoint asociat: /misca_inainte. 
Implementare: Trimite comanda "inainte" către Arduino, care ajustează PWM pentru motoare.
Monitorizarea ambientală prin Senzor MQ-9 ( cel de gaz)
Indicator "Date Gaz" :
Afișează valorile PPM în timp real (actualizat la 2 secunde). Endpoint: /gas_data. Implementare: Categorii "sigur" (<100 PPM), "avertizare" (100-300), "pericol" (>300), cu notificări push pentru alerte.
<img width="433" height="874" alt="image" src="https://github.com/user-attachments/assets/41c1ead4-6bb5-43dd-8039-03fd74e58e9d" />

2.Procesare imagine si flux video
Interfața include un viewer pentru fluxul live de la Camera Module v3, cu butoane pentru selectarea modurilor de procesare (folosind OpenCV pe Raspberry Pi).
ex: Buton "Flux Cameră":
Deschide dialogul pentru vizualizarea fluxului video în timp real. Implementare: Încarcă URL-ul /flux_camera într-un WebView, cu reîmprospătare la 5 secunde pentru continuitate. Fluxul este optimizat la 30 FPS, cu bufferizare pentru latență minimă. Feedback: Afișare live a imaginii procesate.
Buton "Mod RGB" (implicit:
Afișează imaginea în culori naturale. Endpoint: /seteaza_mod?mode=rgb.
Buton "Mod Alb/Negru":
Convertește la grayscale folosind formula ITU-R BT.709. Endpoint: /seteaza_mod?mode=alb_negru. Implementare: cv2.cvtColor pentru tonuri de gri. ( Util pentru condiții de lumină slabă.)
<img width="284" height="498" alt="image" src="https://github.com/user-attachments/assets/de7c2d60-59e6-4dcc-9620-2013daea5891" />

3.Control prin Gesturi
butonul switch care activeaza "Recunoaștere Gesturi":
Activează/dezactivează modul gesturi. Endpoint: /activeaza_mod_gest sau /dezactiveaza_mod_gest. Implementare: Folosește MediaPipe pentru detectarea mâinilor. Gesturi recunoscute: Două degete (înainte), pumn (înapoi), un deget (stânga/dreapta), palmă (oprire)
<img width="426" height="867" alt="image" src="https://github.com/user-attachments/assets/f253f620-a829-4da2-9e7a-cf2c895c1952" />

4.Control prin comenzi vocale
Butonul "Control Vocal":
Deschide dialogul vocal. Implementare: Deschide un dialog cu buton microfon.
Buton "Microfon" (Activare Ascultare):
Pornește recunoașterea vocală. Implementare: SpeechRecognizer în română ("ro-RO"), cu comenzi precum "înainte", "stânga", "oprește". Mapate la endpoint-uri similare controlului de la butonul 1.
<img width="424" height="863" alt="image" src="https://github.com/user-attachments/assets/7d8fc762-2217-49a2-a88b-2a66329d2c09" />

5.Funcționalități speciale (captură și transmitere)
Buton "captură fotografie":
Capturează o imagine. Endpoint: /captureaza_fotografie. 
Implementare: Salvează JPEG local pe telefon și pe Raspberry Pi; apoi ai opțiunea de trimitere aceea fotografie prin email via SMTP/JavaMail. 
Feedback: Mesaj "Fotografie capturată și salvată".
Buton "Înregistrează Video" (Start/Stop):
Pornește/oprește înregistrarea. Endpoint: /porneste_inregistrare și /opreste_inregistrare. 
Implementare: Salvează MP4 local; Feedback: Mesaj "Înregistrare pornită/oprită".
<img width="423" height="864" alt="image" src="https://github.com/user-attachments/assets/2039254e-bd06-4757-8c05-829a1df78f6a" />


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
