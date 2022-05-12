# ARP Poissoning

### Na początku usuwamy adresy z tablicy ARP
```
arp -d <ip>
```

### Wyświetlamy tablicę ARP
```
arp -a
```

### Pingujemy ofiarę z atakującym, dzięki czemu maszyny uzyskują swoje adresy MAC. Za pomocą programu Wireshark można sprawdzić jak wygląda wymiana pakietów (ARP Request oraz ARP Reply).

### Ponownie sprawdzamy tablicę ARP - na tablicy pojawią się adresy pingowanych maszyn.

### Jeżeli maszyna C (firewall) jest stosowana podczas testów to maszyna C będzie w stanie podsłuchiwać ruch sieciowy.

### Teraz uruchamiamy ettercap
```
ettercap -Tq -M arp /ip ofiary// /ip atakującego//
```

### Po uruchomieniu ataku, za pomocą Wiresharka możemy zauważyć podwojoną ilość wysyłanych pakietów podczas pingowania ponieważ część pakietów trafia do podsłuchu.

