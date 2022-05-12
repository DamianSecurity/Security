# Skanowanie sieci
Testy przeprowadzane będą na 2 maszynach wirtualnych (ofiara i atakujący).


### Dla wygody możemy dodać sobie uprawnienia roota oraz odświeżamy listę pakietów w repozytorium:
```
sudo su
apt update
```
### Na maszynie ofiary instalujemy apache2, uruchamiamy i sprawdzamy status usługi.
```
apt-get install apache2
service apache2 start
service apache2 status
```
