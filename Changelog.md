
---
---

03022026
	Zusammenfassung
		DHCP funktioniert jetzt mit allen Geräten, alle Geräte können sich gegenseitig pingen und die Switches waren vorher Layer 2 Switches, wir müssen aber Layer 3 Switches verwenden, also habe ich diese ausgetauscht. Details folgen...
	- Layer 2 Switches durch Layer 3 Switches ersetzt (weil Aufgabenstellung - Unterschied: Layer 3 hat IPs, Layer 2 nicht)
		- Es kann immer nur das Interface des VLANs gepingt werden (192.168.10.1 für VLAN10, ... 20.1 für VLAN20 und ... 30.1 für VLAN30 bei Switch0 im 1.Stock; dieselben Adressen mit Endung auf ... .2 für Switch1 im EG, siehe Notizen in .pkt)
		- DHCP vergibt nur den anderen Servern IPs (wahrscheinlich weil diese im selben VLAN sind, wie der DHCP und die Switches, die ich einfach mal dem VLAN für die Server, also VLAN 30, zugewiesen haben)
		- DHCP direkt mit dem Switch verbunden statt via den Router (hat mein Bruder, Informatiker, mir so geraten)
		- Ich habe den Interfaces für VLANs 10 und 20 auf Switch0 und Switch1 IP Helper-Adressen gegeben (beide sind 192.168.30.10, die des DHCP Servers), damit sie den DHCP erreichen können (leitet DHCP Discovers und -Requests automatisch auf die angegebene Addresse weiter, glaube ich). VLAN 30 braucht keine Helper-Address, da es im selben VLAN ist, wie der DHCP, somit wird der auch so via BC gefunden. 
			Wir könnten auch statt dem Server den Switch als DHCP Server verwenden, aber in der Aufgabenstellung steht explizit, wir sollen dafür einen Server nehmen (sprich nicht den L3 Switch, der das auch könnte, dann hätten wir dieses Problem nicht).
		- Die Switches haben die IPs 192.168.30.20 (Switch0, 1.Stock) und 192.168.30.30 (Switch1, EG)
		- Die Verbindungen wurden grösstenteils über dieselben Ports verkabelt wie bei der Version vom 1.2. (von Näthi); Änderungen: Switches0 und 1 sind jetzt via Gig0/2 zu Gig0/2 verkabelt; DHCP Server0 ist jetzt via Fa0/5 mit Switch0 verkabelt

04022026
	added etherchannel in two separate ways; in the base file, I used ports fa23-24 and gig0/2 as trunk (unneccessary) and in the new file from 04022026 i used fa21-22, instead, where i did not have remnants of l3 config that prevented me from trunking (couldn't get rid of them, so i used different ports)

05022026
	- saved startup-config of the switches with "copy running-config startup-config"
		Solution to task 7 - Resilience; in case of a power outage, nothing will work, but it will work again once power is restored as settings are saved even when the switches are shut down.
	- tried to reset port settings to default for fa0/23-24 on both switches, but it didn't do anything. command: Switch'#': default interface fa0/24 or fa0/23, respectively
		layer 3 settings keep me from allowing trunking between them, but the command doesn't reset their settings and I don't know what else to try, so I'll just keep using fa0/21-22, instead, where everything seems to work just fine.
	- updated documentation, added page numbers and changed the names in the header

To-Do:
	- DNS dokumentieren
	- Korrekturlesen
