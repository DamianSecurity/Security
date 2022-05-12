# Skanowanie sieci
Testy przeprowadzane będą na 2 maszynach wirtualnych (ofiara i atakujący).


### Dla wygody możemy dodać sobie uprawnienia roota oraz odświeżamy listę pakietów w repozytorium:
```
sudo su
apt update
```

### Na maszynie ofiary instalujemy apache2, uruchamiamy i sprawdzamy status usługi:
```
apt-get install apache2
service apache2 start
service apache2 status
```
W tym momencie serwer Apache2 działa, możemy to sprawdzić wchodząc w przeglądarce na localhost.

### Na maszynie atakującego instalujemy program nmap:
```
apt-get install nmap
```

### Uzyskujemy ip ofiary na maszynie ofiary:
```
ip addr show
```

### Puszczamy skan za pomocą programu nmap na port 80 maszyny ofiary.
```
nmap -sT -p 80 -O -v <ip ofiary>
```
Ze skanu można wywnioskować, że status portu 80 jest otwarty oraz jest to port HTTP. Mamy tam również inne interesujące informacje.

