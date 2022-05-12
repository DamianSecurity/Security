# Firewall w systemie Linux
Testy przeprowadzane będą na 2 maszynach wirtualnych (ofiara i atakujący).

### Ofiara uruchamia usługi WWW oraz ssh. Atakujący może sprawdzić dostępność usług za pomocą:
```
nmap -sT -Pn -n -p 80,22 -v <ip ofiary>
```

### Modyfikujemy domyślną politykę ruchu przychodzącego tak aby jakikolwiek ruch przychodzący był odrzucany:
```
iptables -P INPUT DROP
```

### Modyfikujemy politykę aby akceptowany był cały ruch ICMP:
```
iptables -A INPUT -p icmp -j ACCEPT
```

### Możemy zmodyfikować politykę tak aby akceptowany był ruch tcp na port 80 (www):
```
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

### Teraz możemy sprawdzić czy wprowadzone polityki działają poprawnie (z maszyny atakującego):
```
ping <ip ofiary>
nmap -sT -n -p 80 -v <ip ofiary>
```

### Otwieramy dwa dowolne porty (ja wybieram 22 oraz 81):
```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -p tcp --dport 81 -j ACCEPT
```

### Teraz z maszyny atakującego skanujemy porty w celu wykrycia otwartych portów:
```
nmap <ip ofiary> -A -p 1-1024
```
Porty które nie są nasłuchiwane przez żadną usługę mają status closed.
Porty ze statusem filtrowanym są blokowane przez regułę firewall lub inny program sieciowy.

W tym momencie atakujący nie dostanie się na serwer WWW ofiary ponieważ początkowo zablokowaliśmy cały ruch wchodzący, a następnie dodaliśmy tylko poszczególne reguły polityki określającej akceptację ruchu.

Przed kolejnymi testami należy usunąć bieżące reguły, a następnie ustalić regułę akceptującą cały ruch INPUT.

## Filtr pakietów bez pamięci
Testy przeprowadzane będą na 3 maszynach wirtualnych (ofiara, atakujący oraz C jako firewall).

### Na początku modyfikujemy routing tak, aby piekiety przechodziły przez maszynę C:
```
route add -host <ip atakującego> gw <ip firewall> -z maszyny ofiary
route add -host <ip ofiary> gw <ip firewall> -z maszyny atakującego
```

### Na maszynie C (firewall) blokujemy przekierowywanie pakietów ICMP oraz umożliwiamy przekazywanie pakietów (forwarding) pomiędzy interfejsami:
```
sudoedit /proc/sys/net/ipv4/conf/all/send_redirects -zamieniamy wartość 1 na 0
sudoedit /proc/sys/net/ipv4/ip_forward -zmieniamy wartość 0 na 1
```

### Na maszynach ofiary oraz atakującego uruchamiam usługi www oraz ssh.

### Na maszynie firewall stusuję politykę aby maszyna ofiary mogła łączyć się z dowolną stroną WWW:
```
iptables -P FORWARD DROP
iptables -A FORWARD -p tcp -s <ip ofiary> --dport 80 -j ACCEPT
iptables -A FORWARD -p tcp -s <ip ofiary> --sport 80 -j ACCEPT
```

### Aby sprawdzić czy wszystko działa poprawnie możemy spróbować puścić nmap na port 22 (port źródłowy to 80):
```
nmap -sS -Pn -n -p 22 <ip atakującego> --source-port 80
Analogicznie robimy w drugą stronę
```

### Resetujemy reguły

### Teraz ustawimy reguły tak, aby stosowanie SSH z maszyny atakującego na maszyne ofiary działało poprawnie, jednak na odwrót już nie:
```
iptables -A FORWARD -p tcp -d <ip ofiary> --dport 22 -j ACCEPY
iptables -A FORWARD -p tcp -s <ip ofiary> --sport 22 --syn -j DROP
iptables -A FORWARD -p tcp -s <ip ofiary> --sport 22 -j DROP
```

### Działanie reguł możemy sprawdzić za pomocą programu PuTTy.
